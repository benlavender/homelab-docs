![Debian Logo](./images/debian-logo.png "Btw I use Arch...") &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Raspberry Pi](./images/pi-logo.png)

# Wireguard on Raspberry Pi

This guide will cover the installation of wireguard to be used as a VPN server. I use Debian Buster 10.9 for this config, where the backports are required.

**Not recommended for people on dynamic WAN IPs…**

## Configure the backports:

1. Create a new file at **/etc/apt/sources.list.d**, i.e */etc/apt/sources.list.d/backports.list* and enter the text below:
```text
deb http://deb.debian.org/debian buster-backports main
```
2. Refresh any available packages:
```bash
sudo apt update
```

## Install and configure the wireguard package:

1. Install the wireguard package:
```bash
sudo apt install wireguard -y
```
2. Create a new wireguard interface:
```bash
ip link add dev wg0 type wireguard
```
3. 

ip address add dev wg0 192.168.0.3/24


umask 077; wg genkey | tee privatekey | wg pubkey > publickey



