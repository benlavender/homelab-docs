# Reflector:

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