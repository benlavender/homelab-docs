# NetworkManager

`NetworkManager`(8) is a network management service that supports numerous network features, for example:

- 802.11
- VPN-types
- Mobile broadband (WWAN)
- 802.3 Ethernet
- 802.1x
- Teaming and Bonding
- Bridging
- Secrets management
- Polkit

> **Note:** It has its own API and command line tool named `nmcli`(1). See [](../../CheatSheet.md#networkmanager) for example commands.

`NetworkManager` is often used in Linux desktop environments due to its D-Bus API that many GUI applets use to interact with it. 

`NetworkManager` has it's configuration files stored at `/etc/NetworkManager/NetworkManager.conf` but it is better to add custom configuration files to `/etc/NetworkManager/conf.d` as these won't get overwritten on package updates as well as this directory is read before the main configuration file. This also applies to `/usr/lib/NetworkManager/conf.d` which some packages may drop configuration files into. Occasionally `/run/NetworkManager/conf.d` is also used but loaded second after `/etc/NetworkManager/conf.d`.

At default `NetworkManager` manages all ethernet devices without their own connection configured on disk; where each connection would be named `Wired connection #` etc. These connections have DHCPv4 and IPV6 features enabled.

## DHCP:

By default `NetworkManager` uses its own DHCP, likely based on `n-dhcp4` but it can be changed for another one.

## DNS:

At default `NetworkManager` updates `/etc/resolv.conf` with the nameservers provided by the connections. There is no cache unless another library is used.

## Configurations files:

Configuration files are in the `key=value` pair style in sections denoted by `[section-name]` with hashes as comments.

> **Note:** Refer to `NetworkManager.conf`(5) main section for more information on the configuration sections.

## Example configuration files:

### Disable automatic management of all ethernet devices:

```plaintext
[main]
no-auto-default=*
```

### Disable automatic management of a specific device by name:

```plaintext
[main]
no-auto-default=eth1
```