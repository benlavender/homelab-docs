# HAProxy build on Debian stable

This is a quick start guide to building a HAProxy load balancer for a stateless HTTP server backend.

Two servers needed for HTTP and one needed for HAProxy:

- **Operating System:** Debian GNU/Linux 11 (bullseye)
- **Disk:** /dev/sda: 5 GiB
- **Memory:** 1GB
- **Swap:** 1GB

## HAProxy build:

1. Install the HAProxy package (mine was `2.2.9-2+deb11u3`).

```bash
$ apt install haproxy -y
```

2. Verify install and version:

```bash
$ /usr/sbin/haproxy -v
```

3. Confirm service is running and is set as enabled:

```bash
$ systemctl status haproxy.service
```

4. At default, haproxy won't be listening on any TCP ports since only the `global` and `defaults` sections have been configured. The default config is below and found at `/etc/haproxy/haproxy.cfg`. Make a backup of this file:

```bash
$ cp /etc/haproxy/haproxy.cfg <backup_dir>
```

**/etc/haproxy/haproxy.cfg:**

```text
global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http
```
5. Since we have two backend HTTP servers we will configure a simple `frontend` and `backend` set of sections. They are described as:

- **frontend**:
    - `frontend` = Name of the frontend pool.
    - `bind` = IP and port to bind this to on HAProxy server.
    - `default_backend` = Associated `backend` section.

- **backend**
    - `backend` = Name of the backend pool. Has to match the `default_backend` previously.
    - `balance` = The load balancing method, we're using round-robin.
    - `cookie` = We're inserting a cookie named `SERVER` that is sent to the client with the name of the server used during the load balancing operation. We are not inserting this cookie again to a client that already provided this cookie using the `indirect` setting and then using the `nocache` to avoid issues with HTTP caches.
    - `default-server` = This setting applies to all `server` directives, we enable health checks on the `server` lists using `check`. We then set the max connections to all servers using the `maxcon #` directive.
    `server` = Each server has with it's IP address:port, the cookie we're wanting to send to the client with it's value.

```text
frontend lab.airnet.org.uk
	bind 192.168.0.24:80
	default_backend web_servers

backend web_servers
	balance roundrobin
	option httpchk OPTIONS /
	cookie SERVER insert indirect nocache
	default-server check maxconn 32
	server LAB-UXSW001 92.168.0.186:80 cookie LAB-UXSW001
	server LAB-UXSW002 192.168.0.27:80 cookie LAB-UXSW002
```

6. Reload HAProxy and it should now be listening on the correct TCP port:

```bash
$ sudo systemctl reload haproxy.service
$ sudo ss -antpl | grep -i :80
```

7. We now need to install a web server, since obviously the backend pool is invalid right now. We use [Lighttpd](https://redmine.lighttpd.net/projects/lighttpd/wiki) with a dead simple static website for testing. Download the binary and configure:

```bash
$ sudo apt install lighttpd -y
```

8. Create a basic HTML page for each web server by creating a new file named `/var/www/html/index.html`:

```html
<!DOCTYPE html>
<html>
<body>

<h1>LAB-UXSW001</h1>

</body>
</html>
```
```html
<!DOCTYPE html>
<html>
<body>

<h1>LAB-UXSW002</h1>

</body>
</html>
```

9. Confirm on each HTTP server that the site is now being served on port 80:

```bash
curl localhost
```

10. You should now be able send GET requests to the HAProxy service. View the `SERVER` cookie in the response headers and view the round-robin mechanism in action:

```bash
# In the absence of DNS I can map a host entry to an IP and port then send the request to that:
curl --resolve lab.airnet.org.uk:80:192.168.0.24 http://lab.airnet.org.uk --HEAD
```