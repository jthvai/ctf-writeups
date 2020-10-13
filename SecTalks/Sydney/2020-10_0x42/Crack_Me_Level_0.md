# Crack Me Level 0 (beginner)

_Solved on 13 Oct. 2020 (Live)._

## Statement

> Flag format : SecTalks{FLAGHERE}

- `crackme_level1.bin`

## Solution

Run [RetDec](https://retdec.com/) to decompile the attached binary.

```shell
retdec-decompiler crackme_level1.bin
```

Open the generated `crackme_level1.bin.dsm`. As always, we find some string literals in `.rodata`.

```x86asm
; ...
; section: .rodata
0x2000:   01 00 02 00 00 00 00 00                            |........        |
0x2008:   50 61 73 73 77 6f 72 64  3a 20 00                  |Password: .     |   "Password: "
0x2013:   25 73 00                                           |%s.             |   "%s"
0x2016:   4f 57 41 53 50 53 79 64  6e 65 79 54 79 74 68 6f   |OWASPSydneyTytho|   "OWASPSydneyTythonGJ#1"
0x2026:   6e 47 4a 23 31 00                                  |nGJ#1.          |
0x202c:   6e 69 63 65 20 74 72 69  20 6d 38 00               |nice tri m8.    |   "nice tri m8"
0x2038:   66 31 72 73 74 20 70 30  73 73 31 62 6c 65 20 66   |f1rst p0ss1ble f|   "f1rst p0ss1ble fl4g am i the flag"
0x2048:   6c 34 67 20 61 6d 20 69  20 74 68 65 20 66 6c 61   |l4g am i the fla|
0x2058:   67 00                                              |g.              |
0x205a:   74 68 69 72 64 20 70 6f  73 73 69 62 6c 65 20 66   |third possible f|   "third possible flag ddd"
0x206a:   6c 61 67 20 64 64 64 00                            |lag ddd.        |
0x2072:   00 00 00 00 00 00                                  |......          |
0x2078:   55 6e 6c 6f 63 6b 65 64  2e 20 54 68 65 20 66 6c   |Unlocked. The fl|   "Unlocked. The flag is the password."
0x2088:   61 67 20 69 73 20 74 68  65 20 70 61 73 73 77 6f   |ag is the passwo|
0x2098:   72 64 2e 00                                        |rd..            |
0x209c:   49 6e 63 6f 72 72 65 63  74 20 70 61 73 73 77 6f   |Incorrect passwo|   "Incorrect password!"
0x20ac:   72 64 21 00                                        |rd!.            |
0x20b0:   01                                                 |.               |
; ...
```

Note specifically, `The flag is the password.`. An inexplicable yet educated guess suggests that out of these strings,
`OWASPSydneyTythonGJ#1` is most likely to be the password. We may check this by executing the binary (I actually forgot
that I could just... _execute_ the binary until I read the C output).

```shell
$ ./crackme_level1.bin
Password: OWASPSydneyTythonGJ#1
Unlocked. The flag is the password.
```

So, in flag format,

```txt
SecTalks{OWASPSydneyTythonGJ#1}
```

(I originally submitted the flag with a lowercase flag prefix -- à la `sectalks{}`, and that also worked.)
