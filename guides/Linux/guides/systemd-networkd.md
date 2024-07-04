# systemd-networkd - Network manager

`systemd-networkd` is a network service provided by `systemd` that manages network configurations and devices as they appear. 

Network device configurations that are loaded by `systemd-networkd` are based on the `systemd.network` (`.network`) files. These are loaded from:

- `/usr/lib/systemd/network`
- `/usr/local/lib/systemd/network`
- `/run/systemd/network`
- `/etc/systemd/network`

Files in `/etc/systemd/network` will override files in the other directories and all files in each directory are loaded in alphanumeric order. See `systemd.network`(5) for more info. It is recommended as per the documentation that all filenames of the `.network` files are prefixed by a number <70.

> **Note:** `systemd-networkd` will also automatically create virtual network devices based on `systemd.netdev` (`.netdev`) files at:
>
>- `/usr/lib/systemd/network`
>- `/usr/local/lib/systemd/network`
>- `/run/systemd/network`
>- `/etc/systemd/network`

`enable systemd-networkd-wait-online.service` is enabled whenever `systemd-networkd` is also enabled as per `/usr/lib/systemd/system/systemd-networkd.service`, which waits for the associated links to be configured or failed, or at least one to be online. The latter then also relys on `network-online.target` to signify that the network is online.

> **Note:** 
> - You can use [`networkctl(1)`](.././../CheatSheet.md) for adhoc management of systemd-networkd.
> - Ensure the systemd-networkd.service unit is loaded.
> - Do not conflict network daemons like NetworkManager, netctl or iwd with systemd-networkd.
> - On inital setup you may need to enable and start the `systemd-networkd` service: with `systemctl enable systemd-networkd --now`

## Configuration files:

We will create all files in `/etc/systemd/network`.

There are a large number of sections within the configuration files that can be used to configure network devices:
`[Match]`&nbsp;&nbsp;`[Link]`&nbsp;&nbsp;`[SR-IOV]`&nbsp;&nbsp;`[Network]`&nbsp;&nbsp;`[ADDRESS]`&nbsp;&nbsp;`[NEIGHBOR]`&nbsp;&nbsp;`[IPV6ADDRESSLABEL]`&nbsp;&nbsp;`[ROUTINGPOLICYRULE]`&nbsp;&nbsp;`[NEXTHOP]`&nbsp;&nbsp;`[ROUTE]`&nbsp;&nbsp;`[DHCPV4]`&nbsp;&nbsp;`[DHCPV6]`&nbsp;&nbsp;`[DHCPPREFIXDELEGATION]`&nbsp;&nbsp;`[IPV6ACCEPTRA]`&nbsp;&nbsp;`[DHCPSERVER]`&nbsp;&nbsp;`[DHCPSERVERSTATICLEASE]`&nbsp;&nbsp;`[IPV6SENDRA]`&nbsp;&nbsp;`[IPV6PREFIX]`&nbsp;&nbsp;`[IPV6ROUTEPREFIX]`&nbsp;&nbsp;`[IPV6PREF64PREFIX]`&nbsp;&nbsp;`[BRIDGE]`&nbsp;&nbsp;`[BRIDGEFDB]`&nbsp;&nbsp;`[BRIDGEMDB]`&nbsp;&nbsp;`[LLDP]`&nbsp;&nbsp;`[CAN]`&nbsp;&nbsp;`[IPOIB]`&nbsp;&nbsp;`[QDISC]`&nbsp;&nbsp;`[NETWORKEMULATOR]`&nbsp;&nbsp;`[TOKENBUCKETFILTER]`&nbsp;&nbsp;`[PIE]`&nbsp;&nbsp;`[FLOWQUEUEPIE]`&nbsp;&nbsp;`[STOCHASTICFAIRBLUE]`&nbsp;&nbsp;`[STOCHASTICFAIRNESSQUEUEING]`&nbsp;&nbsp;`[BFIFO]`&nbsp;&nbsp;`[PFIFO]`&nbsp;&nbsp;`[PFIFOHEADDROP]`&nbsp;&nbsp;`[PFIFOFAST]`&nbsp;&nbsp;`[CAKE]`&nbsp;&nbsp;`[CONTROLLEDDELAY]`&nbsp;&nbsp;`[DEFICITROUNDROBINSCHEDULER]`&nbsp;&nbsp;`[DEFICITROUNDROBINSCHEDULERCLASS]`&nbsp;&nbsp;`[ENHANCEDTRANSMISSIONSELECTION]`&nbsp;&nbsp;`[GENERICRANDOMEARLYDETECTION]`&nbsp;&nbsp;`[FAIRQUEUEINGCONTROLLEDDELAY]`&nbsp;&nbsp;`[FAIRQUEUEING]`&nbsp;&nbsp;`[TRIVIALLINKEQUALIZER]`&nbsp;&nbsp;`[HIERARCHYTOKENBUCKET]`&nbsp;&nbsp;`[HIERARCHYTOKENBUCKETCLASS]`&nbsp;&nbsp;`[HEAVYHITTERFILTER]`&nbsp;&nbsp;`[QUICKFAIRQUEUEING]`&nbsp;&nbsp;`[QUICKFAIRQUEUEINGCLASS]`&nbsp;&nbsp;`[BRIDGEVLAN]`

> **Note:** See `systemd.network`(5) for more information on the configuration files and syntax.

### Basic wired network using DHCP:

```plaintext
[Match]
Name=eth0

[Network]
DHCP=yes
```

### Basic wired network using static DNS and IP settings:

```plaintext
[Match]
Name=eth0

[Network]
Address=192.168.0.2
Gateway=192.168.0.254
DNS=192.168.0.253
```

### Basic wired network using DHCP that matches all ethernet link types:

> **Note:** Use `networkctl list` and view the `TYPE` field for active links.

```plaintext
[Match]
Type=ether

[Network]
DHCP=yes
```

### Set a specific link device with static settings based on it's MAC address:

```plaintext
[Match]
MACAddress=00:15:5d:01:04:69

[Network]
Address=192.168.0.2
Gateway=192.168.0.254
DNS=192.168.0.253
```

### Set all interfaces to DHCP:

> **Note:** Any empty `[Match]` section will match all interfaces except systemd-networkd will log: "No valid settings found in the [Match] section, ignoring file. To match all interfaces, add Name=* in the [Match]". Use `Name=*` to match all interfaces.

```plaintext
[Match]
Name=*

[Network]
DHCP=yes
```

### Set a link device MAC address matching its permanent MAC address:

> **Note:** Bring the link device down with `ip link set dev <dev> down` before applying the configuration then back up with `ip link set dev <dev> up`.

```plaintext
[Match]
PermanentMACAddress=00:15:5d:01:04:69

[Link]
MACAddress=00:15:5d:01:04:70
```

### Set a link device MAC address matching its name:

> **Note:** Bring the link device down with `ip link set dev <dev> down` before applying the configuration then back up with `ip link set dev <dev> up`.

```plaintext
[Match]
Name=eth1

[Link]
MACAddress=00:15:5d:01:04:70
```