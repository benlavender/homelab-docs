# 1 - Network Name Resolution manager

`systemd-resolved` is a caching and validating stub resolver (plus other features like, DNS-over-TLS, LLMNR and MDNS) that runs under the `systemd`(1) init system. This is offered via fully-featured API exposed via D-BUS, the glibc Name Service Switch `nss`(5) and a local DNS stub listener on both `127.0.0.53` and `127.0.0.54`.

> **Note:** The stub resolver listening on `172.0.0.54` provides a forwarding service only and doesn't perform recursion, where via `172.0.0.53` a fully-featured resolver is available.

## Configuration:

The primary configuration file for `systemd-resolved` is `/etc/systemd/resolved.conf` (see `resolved.conf`(5) for more locations). If using `systemd-networkd` for network configuration then any `.network` files will be read from `/etc/systemd/network`. Any dynamic link configurations from DHCP are also read or information provided by `resolvectl`(1) as well as the current state of `/etc/resolv.conf`.

For system administrators the `/etc/systemd/resolved.conf` is to be used, for programmes they need to use drop-in files at the `/usr` directories documented via `resolved.conf`(5).

> **Note:** To disable a config file use a symlink to `/dev/null`.

### Override examples:

#### Servers:

```plaintext
[Resolve]
DNS=10.0.0.1 10.0.0.2 10.0.0.3:5053 2620:fe::9
```

#### Fallback servers:

```plaintext
[Resolve]
FallbackDNS=1.1.1.1 2620:fe::9
```

#### Search lists:

> **Note:** Search lists are ordered.

```plaintext
[Resolve]
Domains=example.com example.org 
```

#### DNS-over-TLS:

> **Note:** Server must support DoT and using the `address#server_name` format

```plaintext
[Resolve]
DNSOverTLS=yes
```

#### DNS-over-TLS downgrade:

> **Note:** Server must be using the `address#server_name` format

```plaintext
[Resolve]
DNSOverTLS=opportunistic
```

#### Disable LLMNR:

```plaintext
[Resolve]
LLMNR=no
```

#### LLMNR resolve only:

```plaintext
[Resolve]
LLMNR=resolve
```

#### Disable MDNS:

```plaintext
[Resolve]
MulticastDNS=no
```

#### MDNS resolve only:

```plaintext
[Resolve]
MulticastDNS=resolve
```

#### Disable DNSSEC validation:

```plaintext
[Resolve]
DNSSEC=no
```

#### Force DNSSEC validation:

> **Note:** No validation done on LLMNR or MDNS.<br>
> **Note:** DNS server must support DNSSEC or queries will fail.

```plaintext
[Resolve]
DNSSEC=true
```

#### DNSSEC downgrade:

> **Note:** No validation done on LLMNR or MDNS.<br>
> **Note:** Validation is opportunistic depending on the server.

```plaintext
[Resolve]
DNSSEC=allow-downgrade
```

#### Disable the cache:

```plaintext
[Resolve]
Cache=yes
```

#### Disable negative caching:

```plaintext
[Resolve]
Cache=no-negative
```

## Hanlding /etc/resolv.conf:

`systemd-resolved` provides four modes for handling `/etc/resolv.conf` (stub, static, uplink, foreign). All of these settings are automatically selected by `systemd-resolved` based on the current configuration.

1. Maintains `/run/systemd/resolve/stub-resolv.conf` with the `172.0.0.53` stub resolver and search lists, `/etc/resolv.conf` then may be a symlink to this file.

To use this mode: Create a symlink from `/etc/resolv.conf` to `/run/systemd/resolve/stub-resolv.conf` and then restart `systemd-resolved` service:

```bash
ln -sf ../run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
systemctl restart systemd-resolved
```

2. A static file `/usr/lib/systemd/resolv.conf` with `127.0.0.53` server only and no search lists, `/etc/resolv.conf` then may be a symlink to this file.

To use this mode: Create a symlink from `/etc/resolv.conf` to `/usr/lib/systemd/resolv.conf` and then restart `systemd-resolved` service:

```bash
ln -sf /usr/lib/systemd/resolv.conf /etc/resolv.conf
systemctl restart systemd-resolved
```

3. Maintains `/run/systemd/resolve/resolv.conf` and may be a symlink from `/etc/resolv.conf`. All known DNS servers are listed in this file along with search lists. 

To use this mode: Create a symlink from `/etc/resolv.conf` to `/run/systemd/resolve/resolv.conf` and then restart `systemd-resolved` service:

```bash
ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
systemctl restart systemd-resolved
```

4. The file `/etc/resolv.conf` is used directly by `systemd-resolved`. This file can be updated by other applications and packages. If this file is empty then `systemd-resolved` will use its own settings, depening on its configuration. This mode will be used if no symlinks are detected as above.