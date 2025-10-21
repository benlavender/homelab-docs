# Firewalld

Firewalld is a dynamic firewall manager; yes another frontend to the netfilter project. It's put together by Redhat and therefore often used on RHEL-based distributions. It is often paired with other software like `NetworkManager` as interfaces can be assigned to connections and then associated with zones.

I've read a number of guides on this firewall and most often start off with complaining about the lack of documentation for this firewall, especially use cases. So thought I'd do my own wiki to add to it.

It comes with both a CLI `firewall-cmd` (1) and a GUI `firewall-applet` (1) and I've documented the commands in the [cheatSheet](../../CheatSheet.md#firewalld).

The `firewalld` daemon comes with a D-Bus interface. It then supports a number of other "backend" services like iptables, nftables and NetworkManager which then in turn interface with netfilter in the kernel.

## Configuration files:

The main configuration file for `firewalld` is located at `/etc/firewalld/firewalld.conf` where its defaults can be changed. See `firewalld.conf`(5) for more information.

## Concepts:

There are three basic concepts in `firewalld`; zones, services and policies.

### Zones:

`firewalld.zones`(5) define the level of trust for a network connection. They are a collection of settings that effectively permit or deny traffic (and how) once associated with an interface or source.

These are the zones installed by default:

- drop
- block
- public 
- external
- dmz
- work
- home
- internal
- trusted

Usually public is the default zone. 

Looking inside a zone as printed by `firewall-cmd --info-zone=public` it shows the following:

```plaintext
  target: default
  ingress-priority: 0
  egress-priority: 0
  icmp-block-inversion: no
  interfaces: eth0 eth1
  sources:
  services: dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

- target: This is the action taken when a packet **does not** match any rule in the zone. These are either drop, reject or accept where default is reject.
- ingress-priority: Allows for ingress zone prioritisation. Zones with a lower number have a higher priority. This allows for custom ordering of zones.
- egress-priority: Allows for egress zone prioritisation. Zones with a lower number have a higher priority. This allows for custom ordering of zones.
- icmp-block-inversion: Reverts the setting where ICMP packets would be blocked; now allowed.
- interfaces: Lists the interfaces assigned to this zone.
- sources: Optional list of source addresses assigned to this zone. 
- services: Lists the services allowed in this zone.
- ports: Optional list of ports allowed in this zone.
- protocols: Optional list of protocols allowed in this zone.
- forward: Whether intra-zone forwarding is allowed for this zone. This allows packets destined between interfaces/sources to be forwarded within this zone. 
- masquerade: Preset to enable or disable IP masquerading for this zone.
- forward-ports: Optional multiple use elements to forward ports from one port to another.
- source-ports: Optional multiple use elements for source port matching.
- icmp-blocks: Optional multiple use elements for for blocking specific ICMP types.
- rich rules: Optional. Previously known as just "rules`, these are custom rules associated with the zone. 

### Services:

Services are pre-defined entries for ports and protocols. For example a serviced named `ssh` typically uses `22/tcp`. `Firewalld` uses these types to simplify selection of common services as well as custom ones.

Looking inside a service as printed by `firewall-cmd --info-service=ssh` it shows the following:

```plaintext
ssh
  ports: 22/tcp 
  protocols:    
  source-ports: 
  modules:      
  destination:  
  includes:
  helpers:
```

- name: The name of the service.
- ports: The ports and protocols used by this service. 
- protocols: Optional multi-use elements of protocols to extend this service. See `protocols`(5) for more information.
- source-ports: Optional multi-use elements of source ports to extend this service.
- modules: Deprecated.
- destination: Optional element to restrict the service to a specific destination ipv4 and or ipv6 address.
- includes: Optional multi-use elements to include other services as part of this service.
- helpers: Optional multi-use elements of helpers.