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
encrypted   UUID="8dd225a6-9f67-4af2-bbde-aa653b1b243b"    none     nofail,tpm-device=auto
```

> **Note:** This method requires the TPM2 to be active and the unwrap of the key. If any of the PCRs do not match it will not provide the key and a recovery or another key lost must be used. If this happens ensure to re-enrol the devices as per step 1.