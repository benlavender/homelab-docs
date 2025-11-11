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

- **Passphrase:** Password of up to 512 characters in length.
- **Keyfile:** Binary file of up to 8192 KiB.

LUKS containers allow for multiple slots of this key whereas plain does not as and per earlier; plain containers are encrypted with a non-salted hash of the key.

## cryptsetup:

`cryptsetup(8)` is a utility for managing `dm-crypt`. This is documented in [dm-crypt](../../CheatSheet.md#dm-crypt). 