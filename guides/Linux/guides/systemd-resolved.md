# systemd-resolved - Network Name Resolution manager

`systemd-resolved` is a caching and validating stub resolver (plus other features like, DNS-over-TLS, LLMNR and MDNS) that runs under the `systemd`(1) init system. This is offered via fully-featured API exposed via D-BUS, the glibc Name Service Switch `nss`(5) and a local DNS stub listener on both `127.0.0.53` and `127.0.0.54`.

> **Note:** The stub resolver listening on `172.0.0.54` provides a forwarding service only and doesn't perform recursion.

## Configuration:

The primary configuration file for `systemd-resolved` is `/etc/systemd/resolved.conf` (see `resolved.conf`(5)). If using `systemd-networkd` for network configuration then any `.network` files will be read from `/etc/systemd/network`. Any dynamic link configurations from DHCP are also read or information provided by `resolvectl`(1) as well as the current state of `/etc/resolv.conf`.