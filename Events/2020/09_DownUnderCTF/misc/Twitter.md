<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# Twitter (sanity-check, 10pt)

_Solved on 18 Sep. 2020 (Live)._

## Statement

> Check out our Twitter! Find the post with the flag! You can give us a follow if you like <3

## Solution

Visit [DownUnderCTF on Twitter](https://twitter.com/DownUnderCTF). Look for a [suspicious looking
tweet](https://twitter.com/DownUnderCTF/status/1287018872457977856), which reads:

> RFVDVEZ7aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1YZlI5aVk1eTk0c30=

Decode the Base64.

```shell
$ echo -n "RFVDVEZ7aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1YZlI5aVk1eTk0c30=" |\
    base64 -d
DUCTF{https://www.youtube.com/watch?v=XfR9iY5y94s}
```

(That's actually not the rick roll.)
