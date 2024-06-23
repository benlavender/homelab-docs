# systemd-networkd - Network manager

`systemd-networkd` is a network service provided by `systemd` that manages network configurations and devices as they appear. 

Network device configurations that are loaded by `systemd-networkd` are based on the `systemd.network` (`.network`) files. These are loaded from:

- `/usr/lib/systemd/network`
- `/usr/local/lib/systemd/network`
- `/run/systemd/network`
- `/etc/systemd/network`

Files in `/etc/systemd/network` will override files in the other directories and all files in each directory are loaded in alphanumeric order. See `systemd.network`(5) for more info.

> **Note:** `systemd-networkd` will also automatically create virtual network devices based on `systemd.netdev` (`.netdev`) files at:
>
>- `/usr/lib/systemd/network`
>- `/usr/local/lib/systemd/network`
>- `/run/systemd/network`
>- `/etc/systemd/network`