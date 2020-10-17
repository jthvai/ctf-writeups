<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# rot-i (beginner, 100pt)

_Solved on 18 Sep. 2020 (Live)._

## Statement

> **Author**: joseph
>
> ROT13 is boring!
>
> **Attached files**:
>
> - `challenge.txt` (sha256: ab443133665f34333aa712ab881b6d99b4b01bdbc8bb77d06ba032f8b1b6d62d)

- `challenge.txt`

## Solution

Open `challenge.txt`.

```txt
Ypw'zj zwufpp hwu txadjkcq dtbtyu kqkwxrbvu! Mbz cjzg kv IAJBO{ndldie_al_aqk_jjrnsxee}. Xzi utj gnn olkd qgq ftk ykaqe uei mbz ocrt qi ynlu, etrm mff'n wij bf wlny mjcj :).
```

We know the flag format to be `^DUCTF{.*}$`, yet for any single value of `n`, naive ROT-N cannot generate `DUCTF` from
`IAJBO`. I guessed the first word of the challenge text to be "You're" (yeah man it's just magic), and noted the title
of the challenge -- rot-**i**, as in, an index for enumeration.

For the first word `Ypw'zj`, note the following:

- the index of rotation starts at 0, as `Y` is `Y` rot-0;
- `p` is `o` rot-1, `w` is `u` rot-2;
- `'` is preserved, however, `z` is `r` rot-4 -- the index still increments for non-alphabet characters.

With the above information, iterate over each character in the string and un-rot only characters in the latin alphabet,
based on their position.

```julia
#!/usr/bin/julia
open(f -> (global str = readline(f)), "challenge.txt", "r")

outstr = ""
for (i,x) ∈ enumerate(str)
  if isuppercase(x)
    x -= (i - 1) % 26
    x < 'A' && (x += 26)
  elseif islowercase(x)
    x -= (i - 1) % 26
    x < 'a' && (x += 26)
  end
  global outstr *= x
end

println(outstr)
```

Run the script.

```txt
You've solved the beginner crypto challenge! The flag is DUCTF{crypto_is_fun_kjqlptzy}. Now get out some pen and paper for the rest of them, they won't all be this easy :).
```

(He wasn't lying. The rest of them were horrifically difficult.)
