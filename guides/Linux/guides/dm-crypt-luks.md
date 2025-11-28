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

## cryptsetup:

`cryptsetup(8)` is a utility for managing `dm-crypt`. This is documented in [dm-crypt](../../CheatSheet.md#dm-crypt).

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