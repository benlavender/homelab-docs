![Arch Logo](./images/arch-logo.png "Btw I use Arch...") &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Pi Logo](./images/pi-logo.png)


# Config for my Arch Linux ARM box:

*Builder system required sudoer*.

## Filesystems and Partitioning:

This device used a 32GB SDCARD, named **/dev/sdb**.

| Disk | Size |
| --- | --- |
/dev/sdb | 32GB

| Device | Partition Table |
| --- | --- |
/dev/sdb | MBR

| Partition # | Sectors | Partition Type | Size |
| --- | --- | --- | --- |
1 | 2048 - 411647| W95 FAT32 | 200M
2 | 411648 - 61067263 | Linux Filesystem | 5.8GiB

Setup the disk as follows:

1. Create the MBR partition:
```bash
sudo fdisk /dev/sdb
# Clear an existing partitions:
o
# Create a new primary partition for the boot partition:
n
p
1
# Confirm the default start and set the end sectors:
+200M
# Set the FS type as W95 FAT32
t
c
```

2. Create the Linux partition:
```bash
n
p
2
# Confirm both sector defaults
# Now write the changes to disk:
w
```

3. Create the filesystems on both partitions:
```bash
# Create the FAT filesystem:
sudo mkfs.vfat /dev/sdb1
sudo mkdir boot
# Mount the new filesystem to boot:
sudo mount /dev/sdX1 boot
# Create the ext4 filesystem:
sudo mkfs.ext4 /dev/sdb2
sudo mkdir root
# Mount the new filesystem to root:
sudo mount /dev/sdb2 root
```

## Build the OS:

1. Download the .iso from a mirror and store in the new root directory, mine was /lib/udev/root:
```bash
curl -L http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-4-latest.tar.gz -o /lib/udev/root/ArchLinuxARM-rpi-4-latest.tar.gz
```
2. Uncompress the .GZ file and sync writes:
```bash
# As root:
bsdtar -xpf /lib/udev/root/ArchLinuxARM-rpi-4-latest.tar.gz -C /lib/udev/root
sync
```
3. Move the /root/boot directory to the new boot parition:
```bash
sudo mv /lib/udev/root/boot/* /lib/udev/boot
```
4. Unmound the two filesystems:
```bash
sudo umount /lib/udev/boot /lib/udev/root
```
5. Connect to the device and initialize the pacman keyring and populate the Arch Linux ARM package signing keys:
```bash
# As root:
pacman-key --init
pacman-key --populate archlinuxarm
```

## Device configuration:

1. Change the hostname:
```bash
hostnamectl set-hostname <hostname>
```
2. Update the **/etc/hosts** file:
```text
127.0.0.1       localhost
::1             localhost
127.0.1.1       <hostname>.localdomain         <hostname>
```

### Network configuration:

**systemd-networkd.service** is used to manage all network connections and will default to calling DHCP on its own.

#### *(Optional)* Install and configure NetworkManager initially for wired connections:

NetworkManager is not installed at default, my preference is to use systemd.networkd...

1. Install NetworkManager:
```bash
# First re-sync:
pacman -Syy
# Install NetworkManager:
pacman -S networkmanager --noconfirm
```
2. Start and enable at system start:
```bash
systemctl start NetworkManager.service
systemctl enable NetworkManager.service
```
3. Confirm running:
```bash
nmcli -t -f RUNNING general
```
4. (*Optional*) NetworkManager should configure the active interface under "Wired connection 1" with DHCP if available. Use the below command to configure a static assignment if needed:
```bash
nmcli connection modify "Wired connection 1" ipv4.method manual ipv4.addresses <ip.addr/cidr> ipv4.gateway <gw> ipv4.dns <dns1> 
```

#### Connect to a local WLAN network:

Configure **systemd-networkd.service** to manage the wireless interface and **wpa_supplicant** to manage the authentication, if using WEP/WPA(2).

To configure authentication and connection at start, then steps 3-7 and 10 are irrelevant as these manually authenticate to a BSSID. Steps 1-2 and from 11 are only needed for connection at boot time.

1. Verify if managed:
```bash
networkctl list <int>
```

If not, create a new file at **/etc/systemd/network** like **/etc/systemd/network/<int>.network** and restart **systemd-networkd.service**:
```text
[Match]
Name=<int>

[Network]
DHCP=yes
```
```bash
systemctl restart systemd-networkd.service
```

2. Install wpa_supplicant:
```bash
# First re-sync:
pacman -Syy
pacman -S wpa_supplicant --noconfirm
```
3. Create and update the **/etc/wpa_supplicant/wpa_supplicant.conf** file:
```text
ctrl_interface=/run/wpa_supplicant
update_config=1
```
4. Start wpa_supplicant in the background using the new .conf file:
```bash
# Use iwconfig to view installed WLAN interfaces.
wpa_supplicant -B -i <int> -c /etc/wpa_supplicant/wpa_supplicant.conf
```
5. Scan for BSSIDs:
```bash
wpa_cli scan -i <int>
wpa_cli scan_results -i <int>
```
6. Add a new network 
```bash
wpa_cli add_network -i <int> 
```
7. Associate the new network with the BSSID:
```
wpa_cli set_network <#> ssid '"SSID"' -i <int>
```
8. (**Optional**) For WPA/WPA2/WEP then a passphrase needs to be generated for the PSK:
```bash
wpa_passphrase <"SSID"> <'password'>
```
9. (**Optional**) Then set the PSK to the network:
```
wpa_cli set_network <#> passphrase -i <int>
```
10. Enable the network:
```bash
wpa_cli enable_network <id> -i <int>
```
11. Create the **/etc/wpa_supplicant/wpa_supplicant-<int>.conf** file from the stdout of step 8.
```text
ctrl_interface=/run/wpa_supplicant
update_config=1

network={
    ssid="SSID"
    psk=passphrase
}
```
12. Edit the name of the systemd service unit from **wpa_supplicant@.service** to **wpa_supplicant@<int>.service**.
```bash
mv /usr/lib/systemd/system/wpa_supplicant@.service /usr/lib/systemd/system/wpa_supplicant@<int>.service
systemctl daemon-reload
```
13. Enable at system start and start the service:
```bash
systemctl enable --now wpa_supplicant@<int>.service
```

## Further OS configurations:

### Configure the date and time:

1. Set the timezone, use timedatectl list-timezones for zone info:
```bash
timedatectl set-timezone <tz>
```

### Configure locales and language:

1. Uncomment the locate from **/etc/locale.gen**, mine is:
```text
en_GB.UTF-8 UTF-8
```
2. Generate the locale:
```bash
locale-gen
```
3. Create the /etc/locale.conf locale config file and set the variable:
```bash
localectl set-locale LANG=en_GB.UTF-8
```
4. Create a persistent locale for ttys:
```bash
localectl set-keymap --no-convert uk
```

### User configurations:

1. Create a new user for general user:
```bash
useradd <user> --groups <group> --create-home --shell /bin/bash --password <passwordstring>
```
2. Disable any accounts where necessary:
```bash
# Disable account:
usermod <user> --lock
# or
## Delete any account:
userdel --force --remove <user>
```

### Arch User Repository (AUR):

1. Install the base-devel group:
```bash
sudo pacman -S base-devel git --noconfirm
```
2. Install and configure the YAY interface:
```bash
git clone https://aur.archlinux.org/yay.git
# Change to the directory:
cd yay
# Create the package:
makepkg -si --noconfirm
```

### Reflector:

1. Install the reflector package:
```bash
sudo pacman -S reflector --noconfirm
```
2. Configure the reflector.service as:
    1. **Country**: GB
    2. **Fastest**: 5
    3. **Latest**: 20
    4. **Protocol**: Https only
    5. **Age**: 12
    6. **Save**: /etc/pacman.d/mirrorlist
3. Update **/etc/xdg/reflector/reflector.conf** with the below:

```text
# Set the output path where the mirrorlist will be saved (--save).
--save /etc/pacman.d/mirrorlist

# Select the transfer protocol (--protocol).
--protocol https

# Select the country (--country).
# Consult the list of available countries with "reflector --list-countries" and
# select the countries nearest to you or the ones that you trust. For example:
--country GB

# Use only the  most recently synchronized mirrors (--latest).
--latest 5

# Sort the mirrors by synchronization time (--sort).
--age 24
 
# Sort by fastest
--fastest 5
```

4. Start and enable the service:
```bash
sudo systemctl enable reflector.service
sudo systemctl start reflector.service
```

### Alternate package configuration:

1. Install packages that are useful for system administration:
```bash
# Install the below packages:
pacman -S sudo man strace traceroute mtr nmap dmidecode tzdata smartmontools bind whois htop --noconfirm
```
2. Configure sudoers:
```bash
visudo
# Add in any configs
```

### Configure LightDM:

Using the Litarvan theme at https://github.com/Litarvan/lightdm-webkit-theme-litarvan

1. Install lightdm and the theme:
```bash
sudo pacman -S lightdm lightdm-webkit-theme-litarvan lightdm-webkit2-greeter --noconfirm
```
2. Update **/etc/lightdm/lightdm.conf** with the below:
```text
greeter-session=lightdm-webkit2-greeter
```
3. Update **/etc/lightdm/lightdm-webkit2-greeter.conf** with the below:
```text
webkit_theme        = litarvan
```
4. Start LightDM and configure at boot:
```bash
sudo systemctl start lightdm
sudo systemctl enable lightdm 
```

## Install and configure any remaining apps and productivity tools:


neofetch gimp firefox visualstudio code
