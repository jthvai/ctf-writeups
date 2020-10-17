# Pretty Good Pitfall (easy, 200pt)

_Solved on 19 Sep. 2020 (Live)._

## Statement

> **Author**: k0wa1ski#6150
>
> PGP/GPG/GnuPG/OpenPGP is great! I reckon you can't find the message, because it looks scrambled!
>
> **Attached files**: flag.txt.gpg (sha256: dad03ac28b7294c8696eeac21d11159c3dcfc8ed226438804fe82b4fb9f6ad87)

- `flag.txt.gpg`

## Solution

Download the attachment, and "decrypt" it with [GNUPG](https://gnupg.org/).

```shell
$ gpg -d flag.txt.gpg
DUCTF{S1GN1NG_A1NT_3NCRYPT10N}
gpg: Signature made Mon 07 Sep 2020 18:46:12 AEST
gpg:                using RSA key 3A83778AE59F8A5068930CE191F31AFE193132C8
gpg: Can't check signature: No public key
```
