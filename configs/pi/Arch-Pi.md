![Arch Logo](./images/arch-logo.png "Btw I use Arch...") &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Pi Logo](./images/pi-logo.png)


# Config for my Arch Linux ARM box:

## Alternate package configuration:

```bash
# Install the below packages:
pacman -S sudo git openssh --noconfirm
```

**OpenSSH Server**:
```bash
# Enable at boot:
sudo systemctl enable sshd
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

### Arch User Repository (AUR):

1. Install the base-devel group:
```bash
sudo pacman -S base-devel --noconfirm --needed
```
2. Install and configure the YAY interface:
```bash
git clone https://aur.archlinux.org/yay.git
# Change to the directory:
cd yay
# Create the package:
makepkg -si --noconfirm
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

