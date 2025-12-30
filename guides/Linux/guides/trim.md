# Trim in Linux

This guide will go through enabling ATA Trim in Linux for use on SSDs. For a further reading as to why you **may want** and **not want** to do this read the below to articles I have wrote:

1. [Solid State Drives](ssds-trim.md).
2. [dm-crypt / LUKS](dm-crypt-luks.md#trim-support)

## fstrim:

On Linux, the software that sends this Trim information is `FSTRIM(8)`. This discards unused blocks by the filesystem so they can be cleared up by the SSD controller. 

Trim support is specific to individual SSDs and filesystems. To list all block devices that support trim run `lsblk --discard` where this prints the follow:

- **DISC-ALN:** Discard Alignment bytes.
- **DISC-GRAN:** Discard Granularity bytes.
- **DISC-MAX:** Discard Max bytes.
- **DISC-ZERO** Discard Zero byres.

Further reading [sysfs-block](https://www.kernel.org/doc/Documentation/ABI/stable/sysfs-block).

When a column has a zero value it signifies no support where a higher number indicates support.

There are two types of Trim supported **Periodic** and **Continuous**, given **Periodic** is the only recommended one amongst a number of distributions we will configure that one only.

### Filesystems:

In my case both `ext4` and the `HighRel 512GB SSD` disk support trim:

```plaintext
NAME          DISC-ALN DISC-GRAN DISC-MAX DISC-ZERO
sda                  0      512B       2G         0
└─to_be_wiped        0        0B       0B         0
nvme0n1              0      512B       2T         0
├─nvme0n1p1          0      512B       2T         0
└─nvme0n1p2          0      512B       2T         0
```

For periodic trim there are no extra configurations needed i.e mount options. 

### fstrim

1. Enable the `fstrim.timer` unit:

```bash
sudo systemctl enable --now fstrim.timer
```

2. You should now see this timer and its run information using `sudo systemctl list-timers`:

```plaintext
NEXT                            LEFT LAST                        PASSED UNIT                             ACTIVATES
Mon 2026-01-05 01:16:00 GMT   5 days -                                - fstrim.timer                     fstrim.service
```

### SystemD Units:

There are two units associated with fstrim: A service executing the process named `fstrim.service` and systemd timer unit triggering it weekly named `fstrim.timer`:

```plaintext
[Unit]
Description=Discard unused filesystem blocks once a week
Documentation=man:fstrim
ConditionVirtualization=!container
ConditionPathExists=!/etc/initrd-release

[Timer]
OnCalendar=weekly
AccuracySec=1h
Persistent=true
RandomizedDelaySec=100min

[Install]
WantedBy=timers.target
```

```plaintext
[Unit]
Description=Discard unused blocks on filesystems from /etc/fstab
Documentation=man:fstrim(8)
ConditionVirtualization=!container

[Service]
Type=oneshot
ExecStart=/usr/bin/fstrim --listed-in /etc/fstab:/proc/self/mountinfo --verbose --quiet-unsupported
PrivateDevices=no
PrivateNetwork=yes
PrivateUsers=no
ProtectKernelTunables=yes
ProtectKernelModules=yes
ProtectControlGroups=yes
MemoryDenyWriteExecute=yes
SystemCallFilter=@default @file-system @basic-io @system-service
```

The process commandline is `ExecStart=/usr/bin/fstrim --listed-in /etc/fstab:/proc/self/mountinfo --verbose --quiet-unsupported` meaning:

- Filesystem mounts information listed in these files are searched and trim is only executed for these.
- Verbose information is printed after completion.
- Error messages related to non-supported operations are suppressed.