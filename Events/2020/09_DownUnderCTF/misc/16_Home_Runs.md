<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# 16 Home Runs (beginner, 100pt)

_Solved on 18 Sep. 2020 (Live)._

## Statement

> How does this string relate to baseball in anyway? What even is baseball? And how does this relate to Cyber Security?
> ¯(ツ)/¯
>
> ```txt
> RFVDVEZ7MTZfaDBtM19ydW41X20zNG41X3J1bm4xbjZfcDQ1N182NF9iNDUzNX0=
> ```
>
> **Author**: Crem

## Solution

It walks like Base64, quacks like Base64, so it must be. Decode it.

```shell
$ echo -n 'RFVDVEZ7MTZfaDBtM19ydW41X20zNG41X3J1bm4xbjZfcDQ1N182NF9iNDUzNX0=' |\
    base64 -d
DUCTF{16_h0m3_run5_m34n5_runn1n6_p457_64_b4535}
```

(Oh, only _now_ do I see the relevance of the title, while doing the writeup.)
