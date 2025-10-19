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

`NetworkManager` has it's configuration files stored at `/etc/NetworkManager/NetworkManager.conf`. Settings can be overridden here but it is better to add custom configuration files to `/etc/NetworkManager/conf.d/` as these won't get overwritten on package updates as well as this directory is read before the main configuration file. This also applies to `/usr/lib/NetworkManager/conf.d/` which some packages and distributions may drop configuration files into. Occasionally `/run/NetworkManager/conf.d/` is also used but loaded second after `/etc/NetworkManager/conf.d/`. 

Configuration files are in the `key=value` pair style in sections denoted by `[section-name]` with hashes as comments.

> **Note:** Refer to `NetworkManager.conf`(5) main section for more information on the configuration sections.

At default `NetworkManager` manages all devices on the system without their own connection configured on disk; where each ethernet connection would be named `Wired connection #` etc. These connections have DHCPv4 and IPV6 features enabled.

## Daemon:

Ensure the `NetworkManager.service` service unit is started and enabled.

## Connections:

Connections are profiles that define how a device(s) should be configured to connect to a network. These are typically stored as `.nmconnection` files in `/etc/NetworkManager/system-connections/`. Permissions to files in this directory are `0600` and owned by `root:root`. This is to protect secrets such as PSKs or private keys.

## Plugins:

Plugins are used to read and write connection profiles to disk, where `keyfile` is the generic plugin that supports all the connection types and capabilities of `NetworkManager`. Usually when additional plugins are required it's for specific VPN types; like OpenVPN or strongSwan.

## Devices:

Devices are physical or virtual network interfaces that `NetworkManager` can manage.

## DHCP:

By default `NetworkManager` uses its own DHCP, likely based on `n-dhcp4` but it can be changed for another one. 

## DNS:

At default `NetworkManager` updates `/etc/resolv.conf` with the nameservers provided by the connections. There is no cache unless another library is used.

## Example configuration files:

> ℹ️ **Note:** Ensure to reload NetworkManager after making configuration changes.

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