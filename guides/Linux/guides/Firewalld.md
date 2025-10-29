# Firewalld

Firewalld is a dynamic firewall manager; yes another frontend to the netfilter project. It's put together by Redhat and therefore often used on RHEL-based distributions. It is often paired with other software like `NetworkManager` as interfaces can be assigned to connections and then associated with zones.

I've read a number of guides on this firewall and most often start off with complaining about the lack of documentation for this firewall, especially use cases. So thought I'd do my own wiki to add to it.

It comes with both a CLI `firewall-cmd` (1) and a GUI `firewall-applet` (1) and I've documented the commands in the [cheatSheet](../../CheatSheet.md#firewalld).

The `firewalld` daemon comes with a D-Bus interface. It then supports a number of other "backend" services like iptables, nftables and NetworkManager which then in turn interface with netfilter in the kernel.

## Configuration files:

The main configuration file for `firewalld` is located at `/etc/firewalld/firewalld.conf` where its defaults can be changed. See `firewalld.conf`(5) for more information.

## Concepts:

There are five basic concepts in `firewalld`; zones, services, policies, interfaces and sources.

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
public (default, active)
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

- name: The name of the zone. If this zone is default there will be a `(default)` after the name. If this zone is also active there will be an `(active)` after the name.
- target: This is the action taken when a packet **does not** match any rule in the zone. These are either `drop`, `reject` or `accept` where `default` is reject.
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
- helpers: Optional multi-use elements of configurations for connection tracking helpers.

### Policies:

Policies apply rules to traffic flowing inter-zone. They are a set of rules that affect traffic in a stateful unidirectional manner. Policies relate to zones by defining ingress and egress zones. Since policies are unidirectional, they affect the traffic flowing from ingress zones to the egress zones, but not reciprocally. For workstations this wasn't really an issue unless inter-zone traffic was required to be filtered; like for hypervisors.

At default, a policy is inactive and only becomes active when the following are true:

- The ingress zones list contains at least one regular or symbolic zone.
- The egress zones list contains at least one regular or symbolic zone.
- If a regular zone is listed, it must have assigned interfaces or sources.

Looking inside a policy as printed by `firewall-cmd --info-policy=`; which in this case is the default `allow-host-ipv6` policy:

```plaintext
allow-host-ipv6 (active)
  priority: -15000      
  target: CONTINUE      
  ingress-zones: ANY    
  egress-zones: HOST    
  services: 
  ports: 
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
        rule family="ipv6" icmp-type name="neighbour-advertisement" accept
        rule family="ipv6" icmp-type name="neighbour-solicitation" accept
        rule family="ipv6" icmp-type name="redirect" accept
        rule family="ipv6" icmp-type name="router-advertisement" accept
```

- name: The name of the policy. If this policy is active there will be an `(active)` after the name.
- priority: Priority ordering of this zone, this is used when multiple policies may match the traffic. Values are between `-32768` and `32767` where `-1` is the default priority and `0` is reserved. Values of `< 0` will execute before all rules in a zone whereas values `> 0` will execute after all rules in a zone.
- target: Action taken when no rules match. These are either `DROP`, `REJECT`, `ACCEPT` or `CONTINUE` where `CONTINUE` passes the packet to the next matching policy or zone.
- ingress-zones: List of ingress zones for this policy.
- egress-zones: List of egress zones for this policy.
- services: Optional list of services allowed in this policy.
- ports: Optional list of ports allowed in this policy.
- protocols: Optional list of protocols allowed in this policy.
- forward-ports: Optional multiple use elements to forwarded ports.
- source-ports: Optional multiple use elements for source ports.
- icmp-blocks: Optional multiple use elements for blocking specific ICMP types.
- rich rules: Optional. These are custom rules associated with the policy.

#### Symbolic zones:

Symbolic zones are special zone names that represent non-zonal traffic. The pre-defined zones represent situational traffic (topology-based) but not traffic to and from the host for example. There are two symbolic zones:

- Host: Represents traffic flowing to or from the host. 
- Any: Applies to all zones other than host. 

### Interfaces and Sources:

Interfaces represent the network interface cards (NICs) either physical or virtual, sources are source IP ranges. Both of these can be assigned to numerous components of `firewalld`.

Sources allow for zone routing based on the source IP address or range instead of the interface associated with the packet.

### Logging:

Logging is controlled either via the command `firewall-cmd --set-log-denied` or via the `LogDenied` option in the main configuration file. The command option will renew the configuration but the update of the config file will not so ensure you issue `firewall-cmd --reload`.

Logs are printed by the kernel so one way of viewing this is via `journalctl --boot --follow` for an interactive view of rule logs. 