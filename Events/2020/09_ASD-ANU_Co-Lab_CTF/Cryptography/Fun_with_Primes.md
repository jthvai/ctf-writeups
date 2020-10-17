<!-- SPDX-License-Identifier: CC-BY-NC-ND-4.0 -->
# Fun with Primes (5pt)

_Solved on 9 Sep. 2020 (Live)._

## Statement

> Connect to the challenges server at port 3141. It will present you with a series of increasingly difficult problems
> relating to prime numbers.

## Solution

### Stage One

[Ncat](https://nmap.org/ncat/) the server.

```shell
$ ncat [ip] 3141
Welcome!
First question, given N,e,p,q and cipher, give me the message as a string
# See below
Enter Response:
```

(For the sake of aesthetics, here's the _pretty-printed_ JSON output.)

```json
{
  "N": 108852292462543076811268290130596920653380230186656421584861804088654994078901322933050005656845922473580531851275107259559562957696136704137566653866635144913920961123679425398616475096703686029639826720346254555763117304185676699222578035358499297117902356104517295011751108394217724679847764489198874919263,
  "e": 65537,
  "p": 9585906036886064541399890682017843466720906490813138274149226206396065023873794345947086772822622643833832822391228231949487521248695184179272945316423429,
  "q": 11355451643661554222044606762624633509623920462726301135229392137226771125358091839049571252263990555731032619527539177893642322031245642789179589280508947,
  "cipher": "28237aeef7a4f3426a9ee8fdaf00463381e3307b5820c404455ea528acbc3da8a67a0e7f9085e155f3a810538422e201e1714bf28826afc4ed561e056dc952a20361e9a575be2a23f08cdea0c706638f6a3e6583514949f6755b96139c050dc5198a59f3c712deea141750e7e45b25a976f4c2167defde1a5f31e914c25e4137"
}
```

Find out [what these numbers mean in RSA terms](https://www.cryptool.org/en/cto-highlights/rsa-step-by-step).

Notably, in RSA, the secret key _d_ is calculated against the
[totient](https://en.wikipedia.org/wiki/Euler%27s_totient_function) of _N_, _φ(N)_, such that _e ⋅ d = 1_
[modulo](https://en.wikipedia.org/wiki/Modular_arithmetic) _φ(N)_. When _N_ is the product of two primes, _φ(N) = (p -
1)(q - 1)_, where _p,q_ are the factors of _N_.

Find _d_, an integer, such that _d = (k ⋅ φ(N) + 1) / e_ for an integer _k_, and raise the cipher to the power of _d_
modulo _N_.

```julia
julia> k = 1
julia> while (k * φ + 1) % e != 0
         global k += 1
       end
julia> d = (k * φ + 1) ÷ e
julia> msg = modiexp(parse(BigInt, "0x" * cipher), d, N)
6613330628876832443324611210485612401222431984204672551575061905793301974394365433228236314770599591263347537839866748012825827923893874902391360294313058356629439583093699587312544744047648911499950776941245226060148695275589376884576990652464393097986232243711785907789256827585218374050269055815216929
```

(My implementation of modular integer exponentiation, `modiexp`, is
[here](https://gist.github.com/jthvai/6366f2bec88fc26adf23292106c26dd5).)

Convert `msg` to a bytestring.

```py
>>> msg.to_bytes(128,'big')
b'\x00\x02i2\xb5\xd9\x86\xa3R;%lg\x14){o~\x08Ie\x13i\xd4NP\x92\x8bD{\xdc\xf6s\xc9\xfc\xf8\xfe\xa6g1\xb1\x81\x10\x8a#\xe2\xbf\x9f!l\x9di\x89_*w\xdf\xed$@Yz\x13I|\xce\x84\x08|D%\xe0\xc8\xd8\xbbVq\x97\x11\x97xp1t{\x00Well done! One down, only four more to go!'
```

The response is at the end of the bytestring.

```txt
Well done! One down, only four more to go!
```

### Stage Two

```txt
Second question, given N,phi(N) = (p-1)(q-1), give me the smaller prime, p, as a decimal integer
```

```json
{
  "N": 96209525345750030152322116402646699115025532091691209543965329966667590598097083851614757364780306972505815121029464207983557239787186893254196169079054824554242909714119727838654659150619253371524244890762268965532470102814654220567341070534498624221327514943860029309913796195423695848829294195589600756719,
  "phi": 96209525345750030152322116402646699115025532091691209543965329966667590598097083851614757364780306972505815121029464207983557239787186893254196169079054804929355382549868821190108696064553466921567845649084230125532913402083910144677373961550649093560721683071053840089420067402943471321292974863920657507296
}
```

Noting that _φ(N) = (p - 1)(q - 1) = p ⋅ q - p - q + 1 = N - (p + q - 1)_, subtract _φ(N)_ from _N_ and add 1, to get
the sum _p + q_. Then, solve the system of simultaneous equations _[N = p ⋅ q, m = p + q]_. Which can be reduced to
solving the quadratic _p^2 - m ⋅ p + N = 0_ after substituting _q_ for _m - p_ in _p ⋅ q_.

Load the quadratic into [SageMath](https://www.sagemath.org/), and find the smaller root.

```py
sage: m = N - phi + 1
sage: p = var('p')
sage: roots = solve(p^2 - m * p + N == 0, p)
sage: min([p.right() for p in roots])
9539446936289725286633185667581303187905778028716007315158020276881374361386727917369233591693395297140667896916841506429247463533256338341310630383828497
```

Alternatively, let Sage solve the simultaneous equations.

```py
sage: m = N - phi + 1
sage: p, q = var('p q')
sage: roots = solve([N == p * q, m == p + q], p, q)[0]
sage: min([p.right() for p in roots])
```

### Stage Three

```txt
Third question, given N,p+q, give me the smaller prime, p, as a decimal integer
```

```json
{
  "N": 111873203764911180283120003902439404964967366118216969360607152618440357517263085651390597775481599596671752029090649290013000848898732382171157207258813709501805485725000715018881725778352838539396008792236637774013118743720596181480219494620207275981837541029481598068201965121348784106576990956310653886073,
  "p+q": 21248174543774603366569267434484268224470516962467123877319611624143982775008832896955284525055900689875422711626703229618270757718151543665558220053299946
}
```

This is the exact same problem as [Stage Two](#stage-two). Let _m = p + q_.

```py
sage: p = var('p')
sage: roots = solve(p^2 - m * p + N == 0, p)
sage: min([p.right() for p in roots])
9625074461275451622955957366788091845972515602066580087705252135516552339756867697018421984859991679140596096423843526568641050706071277294782001174349189
```

### Stage Four

```txt
Fourth question, given N,q-p, give me the smaller prime, p, as a decimal integer
```

```json
{
  "N": 152029885763238342158449685938901573661961597385046828360568190579954782995507473697302761881616775974388258293466171872712683905509375702570334637054767215021520267436416692377409501046337069315021603419971112988247614734671896579519513408222509995303621705876619374215102134067173783219413563844639802046133,
  "q-p": 856994711221967175127527701768956581937954417995240682199027923071595451865846581063812787032803802313699507178347357309082733944795651154554424529225692
}
```

Let _m = q - p_ this time. Manipulate the values for a different quadratic equation: _p^2 + m ⋅ p - N = 0_, and solve it
in Sage. As one of the roots is negative, take the absolute value of all solutions.

```py
sage: p = var('p')
sage: roots = solve(p^2 + m * p - N == 0, p)
sage: min(map(abs, [p.right() for p in roots]))
11908986007984849358440385398703737929112609907856578181327012408980196465097276790225056159187809177858582435961675429925483760130430999361475124922663111
```

### Stage Five

```txt
Final question, given N,e,d, give me the smaller prime, p, as a decimal integer
```

```json
{
  "N": 151657119808308449300290118218661650655163560849110054201552208033482938674862312273827822256653319286460770023221907167496786059543854445620212488961827646146604274867305403229505892022805561454641964412052680165674518820249903209154009965920674283725772942330003745791744805650534496343205309403027923976259,
  "e": 65537,
  "d": 3110622558318906571241198526039351222509030113867032593514399182122866289421724884651704793833155921638162659161048352520757769993136279392833786328435659864671410802059383271507210404800152600254497727921010648301304657087874556999425044459643837126894844653123767107971048940093221217644931970022454188097
}
```

Refer to [this page](https://www.di-mgt.com.au/rsa_factorize_n.html). (I got incredibly lucky with my Googling
:sweat_smile:.)

The algorithm to find _p_ and _q_ given _N,e,d_ is as follows:

> Input: _N,e,d_. \
> Output: _p_ and _q_ where _p ⋅ q = N_.
>
>   1. Set _k <- e ⋅ d - 1_.
>   2. Choose _g_ at random from _{2,...,N - 1}_ and set _t <- k_.
>   3. If _t_ is divisible by 2, set _t <- t/2_ and _x <- g^t mod N_. Otherwise go to step 2.
>   4. If _x > 1_ and _y = gcd(x - 1,N) > 1_ then set _p <- y_ and _q <- N/p_, output _(p,q)_ and terminate the algorithm. Otherwise go to step 3.

Along with a note:

> Note that we cheat slightly by just choosing small primes _g = 2,3,5,7,11,..._ instead of random values for _g_. We should get
> a result within a few tries.

Implement the algorithm, iterating over the prime numbers for values of _g_.

```julia
#!/usr/bin/julia
function findp(N, kφ)
  # kφ = e * d - 1
  max = 10^5
  primes = erato(max)

  t = kφ
  while t & 1 == 0
    x = modiexp(2, t, N)
    p = gcd(x - 1, N)
    if x > 1 && p > 1
      return min(p, N ÷ p)
    end
    t >>= 1
  end

  for g ∈ 3:2:max
    if primes[g]
      t = kφ
      while t & 1 == 0
        x = modiexp(g, t, N)
        p = gcd(x - 1, N)
        if x > 1 && p > 1
          return min(p, N ÷ p)
        end
        t >>= 1
      end
    end
  end
end
```

(As it turns out, having an upper bound of 10^5 was a slight overkill, as the algorithm terminated at _g = 3_.)

`erato`, found [here](https://gist.github.com/jthvai/de9c2a5d6306016b684c5343f0eebacd) returns an array of booleans,
each of which are true when its index is prime.

Feeding the output of `findp` to the server yields the flag.

```txt
SUCCESS Here is your flag: b'flag{g0ldb4ch_sayz_t0_5ay_Hi!}'
```
