# dm-crypt / LUKS

`dm-crypt` is a block-level encryption `target` for the Linux `Device Mapper` framework. In general `dm` is used for mapping physical block devices to higher level virtual block devices, usually for logical volume manager (LVM), encryption or RAID. `dm-crypt` provides the transparent encryption and decryption of block devices using the kernel crypto API.

## Encryption types:

There are two main types of encrypted volumes provided by `dm-crypt`:

- **plain:** Provides basic encryption. Plain encrypts the device sector-by-sector with a single non-salted hash of the passphrase. Plain does not provide any headers or metadata on the encrypted device and the encryption key cannot be changed or revoked.
- **LUKS:** LUKS provides a header at the start of the device then a key slot area directly behind then the rest of the encrypted data, named a LUKS container/device. With LUKS; multiple passphrases can be used where each can be changed or revoked, these individual passphrases are stored in key slots. The LUKS header can also be detached and stored on a separate device for adaptive security measures. The main characteristics of LUKS is that it stores all the necessary setup information in the volume for ease of use.

There is also limited support for the below encryption types:

- loop-AES
- TrueCrypt
- VeraCrypt
- BitLocker
- FileVault2

There is two types of keys used by `dm-crypt` that protect the master key:

- **Passphrase:** Password of up to 512 characters in length (usually via stdin).
- **Keyfile:** Binary file of up to 8192 KiB.

LUKS containers allow for multiple slots of this key whereas plain does not as and per earlier; plain containers are encrypted with a non-salted hash of the key.

## Managing encrypted volumes:

Managing, including creating volumes, is usually done by `cryptsetup(8)`. Typically these encrypted volumes need to be opened before exposing the encrypted data. This can be done manually or automatically using a few methods: 

- `cryptsetup(8)`: A utility for managing `dm-crypt`. This is documented in [dm-crypt](../../CheatSheet.md#dm-crypt).
- `crypttab(5)`: A file similar to `/etc/fstab` that is used for setting up encrypted volumes at boot.
- `systemd-cryptenroll(1)`: Systemd service unit for enrolling hardware devices like FIDO2 tokens and TPMs that can be used to unlock LUKS2 volumes.

## Disk preparation:

When preparing a disk for block-level encryption, we need to consider securely erasing the data, even for disks that we may have just acquired new from factory and especially when re-using disks that were used previously to store data. Furthermore, before creating an encrypted container the device should be filled with random data.

### Wiping disks:

Typically wiping a disk with all zeros across all its user data areas is satisfactory for removing remanent data. However, different operations need to be performed on different types of disks. These may be cell-based (flash) drives like SSDs or spindle/magnetic disks like HDDs.

For completion times I've ran these on two types of disks for testing:

- **HDD:** Hitachi HTS542525K9SA00.
- **SSD (SATA):** Dogfish SSD 128GB (Branded Gamerking).

#### Hard disk drives:

To fill a HDD with zeros or ones there is two methods, a userland application that can write zeros to the disk or the ATA Security Erase feature (depending on controller support). 

The ATA Security Erase feature is noted a number of times further on and has has two functions: **Security Erase** and **Security Erase Enhanced**:

1. The **Security Erase** method results in all user areas being written with zeros. 
2. The **Security Erase Enhanced** method is known to apply predetermined data patterns, potentially random, to all user areas on the disk. 

It is noted throughout this guide that if the calculated minute numbers are the same then the function likely is exactly the same and the manufacturer didn't implement it. 

On a HDD, there are areas of the disk, both on and off the platters, that are used for operating and managing the disk itself. These areas are not directly accessible to the OS and the host writeable area is separate. Not all HDDs will have these areas (depending on the ATA version) but usually they are used for these purposes when they do:

- **Disk Firmware Area:** Also known as the Service Area, usually the very first part of the platter(s) have reserved manufacturer storage although they may exist elsewhere and can be duplicated for resiliency. The DFA is used by the firmware for a number of uses (called Modules):
    - **P-LIST:** Manufacturer configured defects/bad blocks.
    - **G-LIST:** Lifetime defects/bad blocks developed during use.
    - **SMART logs:** Disk stats like temperature and failure pointers etc.
    - **Security modules**
- **Host Protected Area (HPA):** Introduced with ATA-4. The HPA usually exists after the user writeable area and stores the boot sector and other disk utilities that may be used by the manufacturer. 
- **Device Configuration Overlay (DCO):** Introduced with ATA-6. The DCO is usually used by the manufacturer to set HDD parameters and enable/disable HDD settings.

Detecting the presence and data in these areas require specific forensic analysis tools and vendor specific functions but they can be viewed and potentially edited by an end user. So if you are paranoid about any potential data leakage a full physical destruction (shredding or grinding) service needs to be arranged unless you have the no-how to access these areas. In-fact any paranoia beyond an erasure of the data area should be followed by this method.

##### ATA Security Erase:

1. Ensure `hdparm` is installed.

2. Confirm the device supports this feature:

```bash
sudo hdparm -I /dev/sda
```

3. Confirm the `Security` feature is supported, in our case this is our output:

```plaintext
106min for SECURITY ERASE UNIT. 108min for ENHANCED SECURITY ERASE UNIT.
```

4. The controller works out the time it takes to perform each type, if the minute values are the same it is safe to consider the enhanced version will do the exact same function as the first.

5. Confirm the drive is not frozen as show in the same `Security` section:

```bash
sudo hdparm -I /dev/sda
```

In our case this device was frozen, as this commonly done by motherboard firmware at boot:

```plaintext
frozen
```

6. Usually the easiest way to unfreeze the device is to sleep the system and wake it back up. In our case we can use `sudo systemctl sleep` then wake it back up.

7. Now the device should show:

```plaintext
not     frozen
```

8. Now we need to lock the device with a user password. When prompted, either enter a desired password or just press return to enter a blank one:

```bash
sudo hdparm --security-prompt-for-password --user-master u --security-set-pass /dev/sda
```

9. I will elect to use the enhanced version. Now issue the erase command with the same password when prompted:

```bash
sudo hdparm --security-prompt-for-password --user-master u --security-erase-enhanced /dev/sda
```

10. Once done the command will complete, my time on this was approx 88min so the device calculation was somewhat accurate. 

##### User tools:

A popular tool I use to zero all the blocks on a HDD is `dd(1)`. On the disk I tested it took approx 280 minutes to zero out all the blocks:

1. First we need to find the physical sector size of the disk:

```bash
lsblk -o NAME,PHY-SeC /dev/sda
```

```plaintext
NAME PHY-SEC
sda      512
```

2. Wipe the disk with zeros: 

```bash
sudo dd if=/dev/zero of=/dev/sda bs=512 status=progress
```

3. Once done the command will complete with "No space left on device". My time on this was approx 90 minutes.

#### Flash drives:

These devices are in a league of their own. Because of their nature to degrade over time due to cell charge trapping, manufacturers employ a number of techniques to balance writes amongst cells. These are summarised as "Wear Levelling". At a data erasure standpoint, erasing data from an SSD sometimes can be troublesome because data is moved around and stored in caches and other areas of the disk by the controller, regardless of where the data was written to originally. These usually cannot be disabled, especially on consumer grade disks. Some SSDs are also self-encrypting drives (SED) which allow for key management and encryption offered by the controller and sometimes erasure commands simply result in this key being wiped and therefore data no longer is accessible, but it still could exist in encrypted form. 

There are a few methods which usually will end up with all data being inaccessible on an SSD. Like the HDD method we want to fill the SSD with zeros or ones. A userland application that can write zeros to the disk or the ATA Security Erase feature (depending on controller support):

##### ATA Security Erase:

1. Ensure `hdparm` is installed.

2. Confirm the device supports this feature:

```bash
sudo hdparm -I /dev/sda
```

3. Confirm the `Security` feature is supported, in our case this is our output:

```plaintext
2min for SECURITY ERASE UNIT. 2min for ENHANCED SECURITY ERASE UNIT.
```

4. The controller works out the time it takes to perform each type, if the minute values are the same it is safe to consider the enhanced version will do the exact same function as the first.

5. Confirm the drive is not frozen as show in the same `Security` section:

```bash
sudo hdparm -I /dev/sda
```

In our case this device was frozen, as this commonly done by motherboard firmware at boot:

```plaintext
frozen
```

6. Usually the easiest way to unfreeze the device is to sleep the system and wake it back up. In our case we can use `sudo systemctl sleep` then wake it back up.

7. Now the device should show:

```plaintext
not     frozen
```

8. Now we need to lock the device with a user password. When prompted, either enter a desired password or just press return to enter a blank one:

```bash
sudo hdparm --security-prompt-for-password --user-master u --security-set-pass /dev/sda
```

9. I elected to use the non-enhanced version. Now issue the erase command with the same password when prompted:

```bash
sudo hdparm --security-prompt-for-password --user-master u --security-erase /dev/sda
```

10. Once done the command will complete, my time on this was about 5 seconds. In this case it is likely that the device is a SED (even if not advertised by the firmware with OPAL support) and the controller likely reset the key. 

##### User tools:

A tool I use to create zeros across all blocks on an SSD is `blkdiscard(8)`. `blkdiscard` is used to discard device sectors on SSDs by using the ATA Trim function (if supported). However it can be used to write zeros across all sectors of the disk if Trim is not supported: 

1. Ensure `util-linux` is installed.

2. Using the `--secure` flag will also ensure that any discarded blocks created by the controller garbage collection routine are also erased. This depends on firmware support:

```bash
sudo blkdiscard --secure --force /dev/sda
```

```plaintext
blkdiscard: BLKSECDISCARD: /dev/sda ioctl failed: Operation not supported
```

3. In this case the operation wasn't supported so an alternative is to use this command to zero all the blocks manually:

```bash
sudo blkdiscard --zeroout --force /dev/sda
```

## Examples:

### Encrypt a partition with LUKS using a passsphrase then create a FAT filesystem:

We will encrypt `/dev/sdh2` using LUKS then create a FAT filesystem on the encrypted partition.

The partition table is as follows:

| Disk | Size |
| --- | --- |
/dev/sdh | 1 GiB |

| Device | Partition Table |
| --- | --- |
/dev/sdh | GPT |

| Partition # | Sectors | Partition Type | Size | Filesystem |
| --- | --- | --- | --- |
1 | 2048 - 1026047 | Linux filesystem | 500M | ext4 |
2 | 1026048 - 1069056 | Linux filesystem | 522M | None |

1. Ensure the partition already exists and is not mounted to a filesystem.
2. Encrypt the second partition with LUKS2 and confirm the passphrase when prompted:

```bash
sudo cryptsetup luksFormat /dev/sdh2
```

3. To open and map this device run the following and pass in the passphrase when prompted:

```bash
cryptsetup open /dev/sdh2 encrypted_part
```

4. The device should now be mapped to /dev/mapper/encrypted_part which can be verified with:

```bash
sudo cryptsetup status encrypted_part
```

5. Now we can create a filesystem on the mapped device:

```bash
sudo mkfs.fat -F32 /dev/mapper/encrypted_part
```

### Encrypt a USB drive with LUKS using a passphrase:

We have a new USB stick that we want to encrypt for when we carry it around.

The partition table is as follows:

| Disk | Size |
| --- | --- |
/dev/sdb | 58 GiB |

| Device | Partition Table |
| --- | --- |
/dev/sdb | GPT |

| Partition # | Sectors | Partition Type | Size | Filesystem |
| --- | --- | --- | --- |
1 | 2048 - 121108479 | Linux filesystem | 57.7G | None |

1. Encrypt the the partition with LUKS2 and confirm the passphrase when prompted:

```bash
sudo cryptsetup luksFormat /dev/sdb1
```

2. To open and map this device run the following and pass in the passphrase when prompted:

```bash
cryptsetup open /dev/sdb1 encrypted_stick
```

3. The device should now be mapped to /dev/mapper/encrypted_stick which can be verified with:

```bash
sudo cryptsetup status encrypted_stick
```

4. Now we can create a filesystem on the mapped device:

```bash
sudo mkfs.fat -F32 /dev/mapper/encrypted_stick
```

### Open a LUKS container at boot:

We have a new disk that we want mounted during boot like the rest of the disks that is encrypted with LUKS. We will use a keyfile to unlock the volume and configure `systemd-cryptsetup(8)` to unlock at boot.

The partition table is as follows:

| Disk | Size |
| --- | --- |
/dev/sdc | 1 GiB |

| Device | Partition Table |
| --- | --- |
/dev/sdc | GPT |

| Partition # | Sectors | Partition Type | Size | Filesystem |
| --- | --- | --- | --- |
1 | 2048 - 2095103 | Linux filesystem | 1G | None |

1. Generate a key file and place it in `/etc/cryptsetup-keys.d/`:

```bash
sudo printf '%s' <'passphrase'> | sudo install -m 0600 /dev/stdin /etc/cryptsetup-keys.d/encrypted.key
```

2. Encrypt the the partition with LUKS2 using the key file:

```bash
sudo cryptsetup luksFormat --key-file /etc/cryptsetup-keys.d/encrypted.key /dev/sdc1
```

3. We will need the partition UUID:

```bash
sudo blkid /dev/sdc1
```

4. Update `/etc/crypttab` to include the new encrypted partition:

```plaintext
encrypted   UUID="8dd225a6-9f67-4af2-bbde-aa653b1b243b"    none     nofail
```

The above options are as follows:

1. First section is the name of the LUKS container once mapped.
2. Second option is the ID of the disk or partition, in this case it is a partition that is encrypted.
3. Third option defines the location of the key file, using either `-` or `none` will result in a key named the same as the first option proceeding `.key` being loaded from `/etc/cryptsetup-keys.d/` and `/run/cryptsetup-keys.d/`
4. The forth section are options defined at `crypttab(5)` and in our case the `nofail` option doesn't delay the boot process if the key isn't retrievable. 

> **Note:** Obviously the key is available on the root partition, if this is not protected beyond simple filesystem permissions then it will likely be retrievable offline.

### Open a LUKS container at boot with a FIDO2 token:

Using the same encrypted container as the above example, we also might want to use a FIDO2 token to unlock at boot. The device must be connected during the setup.

1. Ensure `libfido2` is installed.

2. Ensure the device is connected to the system.

```bash
sudo systemd-cryptenroll --fido2-device=list
```

3. Enrol this device into a key slot on the existing container:

```bash
sudo systemd-cryptenroll --fido2-device=auto --fido2-with-client-pin=no --unlock-key-file=/etc/cryptsetup-keys.d/encrypted.key /dev/sdc1
```

The above options are as follows:

- `--fido2-device=auto` auto selection of the FIDO2 token.
- `--fido2-with-client-pin=no` does not prompt the token user for a PIN. This only speeds up the process but technically weakens security; change to `yes` or do not include if you want to be prompted.
- `--unlock-key-file=` this is the key file used to unlock and enrol the token.

> **Note:** The `--fido2-with-user-presence=no` in the man page isn't compliant when using hmac secrets in this case, so most tokens are going to require some presence.

4. Update `/etc/crypttab` to include the fido2 token:

```plaintext
encrypted   UUID="8dd225a6-9f67-4af2-bbde-aa653b1b243b"    none     nofail,fido2-device=auto
```

> **Note:** (Optional) Make a backup of the FIDO2 token by enrolling multiple devices, however the missing token will, in this case, fallback to the key stored on disk so the risk pointer mentioned in the previous example notes still applies.

### Open a LUKS container at boot with a TPM:

We have a device that has a TPM installed. We want to use this as a key slot in a LUKS2 container.

Again we will use the same container as the previous two examples by enrolling the TPM device.

1. Enroll the TPM2 device using the existing key file with a number of PCRs:

```bash
sudo systemd-cryptenroll --tpm2-device=auto --tpm2-pcrs=platform-code+external-code+system-identity --unlock-key-file=/etc/cryptsetup-keys.d/encrypted.key /dev/sdc1
```

The above options are as follows:

- `--tpm2-device=auto` auto selection of the TPM2 device.
- `--tpm2-pcrs` these are the [PCRs](https://uapi-group.org/specifications/specs/linux_tpm_pcr_registry/) used. See `systemd-cryptenroll(1)` for explanation. We should have enough to cover main EFI firmware changes and a number of things on root (if not encrypted).
- `--unlock-key-file=` this is the key file used to unlock and enrol the token.

2. Update `/etc/crypttab` to include the fido2 token:

```plaintext
encrypted   UUID="8dd225a6-9f67-4af2-bbde-aa653b1b243b"    none     nofail,tpm2-device=auto
```

> **Note:** This method requires the TPM2 to be active and the unwrap of the key. If any of the PCRs do not match it will not provide the key and a recovery or another key lost must be used. If this happens ensure to re-enrol the devices as per step 1.

### Trim support:

The ATA Trim command is not and never will be enabled by default until security issues are resolved with this function in SSDs. These are cell block devices i.e NAND for example. 

None of the examples so far have enabled trim (discard). When Trim is used on SSDs the operating system, which in Linux is `fstrim(8)`, informs the SSD controller of the file deletions so that the controller can mark the pages as unused (INVALID) for it's own garbage collection schedule. If this Trim was disabled, which is recommended for LUKS2, garbage collection would have no idea what pages contain valid data because the controller has no knowledge of the filesystem; it only knows about invalid and valid pages. Therefore when running, usually at idle, garbage collection may relocate both valid and deleted data to new blocks before erasing the previous block.

The issue at a security standpoint with Trim is that it informs the controller what exactly is valid and not valid data, which might be the cyphertext. This can lead to the storage profile forming patterns where potentially both the filesystem and the type of files are identified. This does not reveal any encrypted data though. Effectively this may provide hints to forensic analysis of what type of data is potentially stored and where it is on the disk; making the encryption and the owner less anonymous.

There are three ways to enable Trim (presuming the disk supports it):

1. When creating the mapping manually just once.
2. When creating the mapping and permanently enabling trim by writing to the header.
3. When creating the mapping at boot with `crypttab(5)`.

> **Note:** `fstrim(8)` is a separate process that isn't documented here. 

#### Enabling Trim manually:

1. When creating a file mapping, you can enable Trim for this mapping only, this then will be removed once the mapping is closed:

```bash
sudo cryptsetup open --allow-discards <dev> <name>
```

2. To store this permanently in the LUKS2 header:

```bash
sudo cryptsetup open --allow-discards --persistent <dev> <name>
```

3. Now then querying the header with `sudo cryptsetup luksDump <dev>` you can see the flag: 

```plaintext
Flags:          allow-discards
```

4. (Optional) This can be removed with `sudo cryptsetup close <name>` & `sudo cryptsetup open --persistent <dev> <name>` if required.

#### Enabling Trim with crypttab:

1. The option `discard` can be used to allow Trim in the mapping:

```plaintext
encrypted   UUID="8dd225a6-9f67-4af2-bbde-aa653b1b243b"    none     discard
```