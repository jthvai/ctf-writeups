# In a pickle (easy, 200pt)

_Solved on 19 Sept. 2020 (Live)._

## Statement

> **Author**: n00bmaster
>
> We managed to intercept communication between und3rm4t3r and his hacker friends. However it is obfuscated using
> something. We just can't figure out what it is. Maybe you can help us find the flag?

- `data`

## Solution

Open the attachment, which is a text file. Note the following line, amongst others.

```txt
...
S"I know that the intelligence agency's are onto me so now i'm using ways to evade them: I am just glad that you know how to use pickle. Anyway the flag is "
...
```

...as in, [Python pickle](https://docs.python.org/3/library/pickle.html). Directly unpickling `data` reveals the
following list:

```py
>>> from pickle import loads
>>> with open("data") as f:
...   global bytestr
...   bytestr = bytes(f.read(), "utf-8")
...
>>> list(loads(bytestr).values())
['D', 'UCTF', '{', 112, 49, 99, 107, 108, 51, 95, 121, 48, 117, 82, 95, 109, 51, 53, 53, 52, 103, 51, '}', "I know that the intelligence agency's are onto me so now i'm using ways to evade them: I am just glad that you know how to use pickle. Anyway the flag is "]
```

Write a script to convert the above codepoints into flag format.

```py
#!/usr/bin/python
from pickle import loads

with open("data") as f:
  global bytestr
  bytestr = bytes(f.read(), "utf-8")

data = [] # Will only contain the converted ints
for c in list(loads(bytestr).values())[3:-2]:
  data.append(chr(c))

print("DUCTF{" + "".join(data) + "}")
```

Run the script.

```txt
DUCTF{p1ckl3_y0uR_m3554g3}
```
