<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# formatting (beginner, 100pt)

_Solved on 18 Sep. 2020 (Live)._

## Statement

> **Author**: h4sh5
>
> Its really easy, I promise
>
> **Files**: [formatting](https://play.duc.tf/files/235a555e84c8fe3cdbd0bb4c90389583/formatting)

- `formatting`

## Solution

Run [RetDec](https://retdec.com/) to decompile the attached binary.

```shell
retdec-decompiler formatting
```

Open the generated `.dsm` file. In the Data Segment section, note the following.

```x86asm
; ...

;;
;; Data Segment
;;

; ...

; section: .rodata
0x2000:   01 00 02 00 00 00 00 00                            |........        |
0x2008:   44 55 43 54 46 7b 68 61  68 61 20 69 74 73 20 6e   |DUCTF{haha its n|   "DUCTF{haha its not that easy}"
0x2018:   6f 74 20 74 68 61 74 20  65 61 73 79 7d 00         |ot that easy}.  |
0x2026:   00 00                                              |..              |
0x2028:   25 73 25 30 32 78 25 30  32 78 25 30 32 78 25 30   |%s%02x%02x%02x%0|   "%s%02x%02x%02x%02x%02x%02x%02x%02x}"
0x2038:   32 78 25 30 32 78 25 30  32 78 25 30 32 78 25 30   |2x%02x%02x%02x%0|
0x2048:   32 78 7d 00                                        |2x}.            |
0x204c:   64 31 64 5f 59 6f 75 5f  4a 75 73 74 5f 6c 74 72   |d1d_You_Just_ltr|   "d1d_You_Just_ltrace_"
0x205c:   61 63 65 5f 00                                     |ace_.           |
0x2061:   ??                                                 |?               |

; ...
```

Unfortunately that was not the flag. Run [ltrace](https://www.ltrace.org/) on the binary, as per the suggestion of the
suspicious string.

```shell
$ ltrace ./formatting
sprintf("d1d_You_Just_ltrace_296faa2990ac"..., "%s%02x%02x%02x%02x%02x%02x%02x%0"..., "d1d_You_Just_ltrace_", 0x29, 0x6f, 0xaa, 0x29, 0x90, 0xac, 0xbc, 0x36) = 37
puts("haha its not that easy}"haha its not that easy}
)                                                     = 24
+++ exited (status 0) +++
```

Notice that there are 6 bytes of hex at the end of the first argument, but 8 bytes worth of hex arguments at the end of
the call to `sprintf`. Concatenate the last two argument bytes with the original string, and place it into flag format.

```txt
DUCTF{d1d_You_Just_ltrace_296faa2990acbc36}
```
