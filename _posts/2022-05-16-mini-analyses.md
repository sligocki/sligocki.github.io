---
layout: post
title: Mini Analyses
tags: busy-beaver collatz
---

I spend a lot of time doing "quick-and-dirty" analyses of new Busy Beaver champions. Recently, when I have time, I have been posting those analyses on this blog in detailed form. However, converting from my quick notes to a polished blog post is a lot of time and work and so most of my analyses don't make it. However, I think there is some value in the quick notes, so I keep them in unsaved documents in my editor until I end up with too many tabs (or my computer crashes) and eventually I delete them. As a middle ground, I've decided to try and post unpolished notes here as a value to myself if I ever come back and want to understand these machines behavior again in the future. I may add more notes here over time.


## Pavel-e197k

Shared by email 15 May 2022 by Pavel Kropitz. This TM runs >10^197,282 steps leaving >10^98,641 symbols.

`1RB 1RH  1RC 1RA  1RD 0RB  1LE 0RC  0LF 0LD  0LB 1LA`

Rule 1:  `11 01^n D> 00  ->  01^n+2 D>`
Rule 1x: `11^n D> 0^inf  ->  01^2n D> 0^inf`

Rule 2:  `1010   01^m D> 0^inf  ->  01^2m+8 D> 0^inf`
Rule 2x: `1010^n 01^m D> 0^inf  ->  01^((m+8) 2^n - 8) D> 0^inf`

Rule 3: `000 01^n D> 000    ->  0100 10^n 11 D>`
Rule 4: `100 01^n D> 0^inf  ->  11 10^n+1 11^1 D> 0^inf`

Rule 5: `000 01^2k+1 D> 0^inf  ->  01^(20 2^k - 8) D> 0^inf`
Rule 6: `000 01^2k D> 0^inf  ->  011 Z> 10 10^(10 2^(5 2^k - 4) - 8) 1 0^inf` (Halt(`10 2^(5 2^k - 4) - 4`))
Rule 7: `0^inf 01^2k+1 D> 0^inf  ->  Halt(10 2^(5 2^(10 2^k - 4) - 4) - 5)`

At step 9, it's in config `0^inf 01^3 D> 0^inf` -> `Halt(10 2^(5 2^16 - 4) - 4)`


# Machine Adjacent to Pavel-e197k

While analyzing 6x2 machines "adjacent" to Pavel-e197k, I found this machine which our filters do not appear to be able to categorize:

`0LD 1RZ  1RC 1RA  1RD 0RB  1LE 0RC  0LF 0LD  0LB 1LA`

Rule 1:  `1010   01^m D> 00     ->  01^m+3  D>`
Rule 1x: `1010^n 01^m D> 0^inf  ->  01^m+3n D> 0^inf`

Rule 2: `0^inf 100 01^m D> 0^inf  ->  0^inf 10010 01^m+1 D> 0^inf`
Rule 3: `0^inf 10010 01^m D> 0^inf ->  0^inf 100 10^m+3 01^2 D> 0^inf`

Rule 4a: `100 01^2k D> -> 100 10^2k+4 01^2 D> -> 100 01^3k+8 D>`
Rule 4b: `100 01^2k+1 D> -> 100 10^2k+5 01^2 D> -> 10010 01^3k+8 D> -> 100 10^3k+11 01^2 D>`

Rule 5a: `100 10^2k 01^2 D> -> 100 01^3k+2 D>`
Rule 5b: `100 10^2k+1 01^2 D> -> 10010 01^3k+2 D> -> 100 10^3k+5 01^2 D>`

At step 20, it's in config `100 01^2 D>` at which point Rules 4/5 a/b will apply forever, so this is infinite (but our system is not smart enough to know that).


# Machine Adjacent to e78k

While analyzing 6x2 machines "adjacent" to the short-lived 6x2 champion I announced on Friday (which runs for >10^78913 steps), I found exactly one machine (with 3 different permutations of start state) which our filters aren't able to categorize. This is the same as my 6x2 former champion except with `A1 -> 1RA`.

`1RB 1RA  1LC 0RF  1RA 0LD  0LC 0LE  1LD 0RA  1RE 1RZ`

a0: `00001 <E 0101^a 1^3k   00  ->  1 <E 0101 1 0^4a 001^k 1`
a1: `  001 <E 0101^a 1^3k+1 00  ->  1 <E 0101^a+1 001^k 1`
a2: ` 0001 <E 0101^a 1^3k+2 00  ->  1 <E 0101 1^4a+2 001^k 1`

Once this machine gets into a tape matching regular expression `0^inf 1 <E (0101)+ (00|1)* 0^inf` it will never leave. Thus these machines are infinite.


# Swapping Rules

`1RB 1RF  1LC 1LB  --- 0LD  1LE 1LF  1RE 1LD  0RF 0RA`

Rule 1:  `0100 01^a <D 01 11^b  ->  0100 01^b+1 <D 01 11^a`
Rule 1x: `0100 01^a <D 01 11^b  ->  0100 01^a+1 <D 01 11^b -> Infinite`


`1RB 1LC  0RC 1LE  1RD 0LA  1LB 1RC  1RC 1LF  --- 1LE`

Rule 1:  `01 11^a 10 C> 01^b 11 -> 01 11^b-2 10 C> 01^a+4 11`
Rule 1x: `01 11^a 10 C> 01^b 11 -> 01 11^a+2 10 C> 01^b+2 11 -> Infinite`


# Three Stage Rules

`1RB 1LE  1LC ---  1RD 1LB  1LF 0LE  0LD 1RF  0RE 0RA`

`C(a, b, c) = <F 10^a 0^b 11^c`

Rule 1:  `C(a, b + 1, c) -> C(a + 1, b, c)`
Rule 1x: `C(a, b, c) -> C(a + b, 0, c)`

Rule 2:  `C(a, 0, c+2) -> C(1, 2a+2, c+1) -> C(2a+3, 0, c+1)`
Rule 2x: `C(a, 0, c+1) -> C( (a+3) 2^c - 3, 0, 1)`

Rule 3:  `C(a, 0, 1) -> C(1, 4, 2a+4) -> C(5, 0, 2a+4) -> C(8 2^(2a+4) - 3, 0, 1)`
Rule 3x: `C(a, 0, 1) -> Infinite`


`1RB ---  1LC 1LB  0RE 0LD  1RD 1LC  1RF 0LE  1RE 1RA`

`E(a, b, c) = <E 0^2a+1 1^b 01^2c+1`

Rule 1: `E(a, b+1, c) -> E(a+1, b, c)`
Rule 1x: `E(a, b, c) -> E(a+b, 0, c)`

Rule 2: `E(a, 0, c+1) -> E(0, 2a+6, c) -> E(2a+6, 0, c)`
Rule 2x: `E(a, 0, c) -> E( (a+6) 2^c - 6, 0, 0)`

Rule 3: `E(2k, 0, 0) -> E(0, 0, k+1) -> E(6 2^k+1 - 6, 0, 0)`
Rule 3x: `E(2k, 0, 0) -> Inf`


`1RB 1LE  1RC 1RB  1LD 0RE  --- 0LA  1LF 0RD  1RB 1LA`

`E(a, b, c) = <E 10^a 1^b 0 01^c`

Rule 1:  `E(a, b+2, c) -> E(a+1, b+1, c)`
Rule 1x: `E(a, b+1, c) -> E(a+b, 1, c)`

Rule 2:  `E(a, 1, c+1) -> E(1, 2a+3, c) -> E(2a+3, 1, c)`
Rule 2x: `E(a, 1, c) -> E((a+3) 2^c - 3, 1, 0)`

Rule 3: `E(a, 1, 0) -> E(1, 1, a + 2) -> E(4 2^(a+2) - 3, 1, 0)`
Rule 3: `E(a, 1, 0) -> Infinite`


`1RB ---  1RC 1RA  0RD 1LE  1RE 0RC  0LC 0LF  1LA 1RC`

E(a, b, c) = 11^a 0^b 01^c E>

E(a, b+1, c) -> E(a, b, c+1)
E(a, b, c)   -> E(a, 0, c+b)

E(a+2, 0, c) -> E(a+1, 2c + 2, 1) -> E(a+1, 0, 2c+3)

E(1, 0, c) -> E(c+3, 0, 1) -> Inf


`1RB 0LC  0LA 1RD  1LA 1LB  0RE 1LA  0RB 0RF  1RE ---`

Rule 1:  00 <C 01^n+1 -> 11^2 00 <C 01^n
Rule 1x: 00 <C 01^n+1 -> 11^2n 00 <C 01
Rule 1+: 00 <C 01^n+1 -> <C 01^2n 001

C(a, b, c) = 11^a 00 <C 01^b 001 01^c 001

Rule 2:  C(a, b+1, c) -> C(a+1, b, 2c)
Rule 2x: C(a, b, c) -> C(a+b, 0, c 2^b)

Rule 3:  C(a, 0, c) -> C(0, a+1, 2c) -> C(a+1, 0, c 2^b+1)
Rule 3x: C(a, 0, c) -> Infinite


# Halting 6x2

`1RB 0LE  0RC 1RF  1RD 0RB  1LA 1RB  1LA 0LD  1RA 1RZ`

```
0 011^n 01 D> 0  ->  1^3n+2 01 D>

D(n, a, b, c) = 111 011^n 0^a 1^b 011^c 01 D>

Rule 1:  D(a, b+3, c) -> D(a, b+1, c+2)
Rule 1x: D(a, 2k+r, c) -> D(a, r, c+2k)

Rule 2: D(a+1, 0, c) -> D(a, 3c+2, 0)

Rule 3: D(a+1, 2k) -> D(a+1, 0, 2k) -> D(a, 6k+2) = D(a, 3b+2)
Rule 3x: D(a, 2k) -> D(0, A 3^a - B)

Rule 4: D(4, 1, c+1) -> 111 011 D(2, 3c+1, 0)
Rule 5: 1 D(2, 1, c) -> 1011 D(4, 0, c) -> 1011 D(3, 3c+2, 0)

@3:  D(0, 1, 1)
@36: D(0, 5, 1)
1x:  D(0, 1, 5)
4:   111 011 D(2, 13)
1x:  111 011 D(2, 1, 12)
5:   111 011 011 D(3, 38)  # 38 -> 116 -> 350 -> 1052
3x:  111 011 011 D(0, 1052)  =  111 011 D(1, 1054)
3x:  111 011 D(0, 3164) = 111 D(1, 3166)
3x:  111 D(0, 9500) = D(0, 9503)
1x:  D(0, 1, 9502)
4:   111 011 D(2, 28_504)
```


```
       +-----+-----+
       |  0  |  1  |
   +---+-----+-----+
   | A | 1RB | 0LE |
   +---+-----+-----+
   | B | 0RC | 1RF |
   +---+-----+-----+
   | C | 1RD | 0RB |
   +---+-----+-----+
   | D | 1LA | 1RB |
   +---+-----+-----+
   | E | 1LA | 0LD |
   +---+-----+-----+
   | F | 1RA | 1RZ |
   +---+-----+-----+




<D 001010  100^4     101
 10 <D 00  100^2     101   001^340    01
  <E 0100  100^9     10    100^e10    101
<D 001010  100^23    10    100^e28    101
 10 <D 00  100^82    10    100^e111   101
  <E 0100  100^279   10    100^e391   101
<D 001010  100^946   10    100^e1344  101
 10 <D 00  100^3195  10    100^e4561  101
  <E 0100  100^10787 10 10 100^e15427 101

A(a, b) = 1011 <A 100^a 100 101 001 001^b 01
  111 011 00 111^a 1101100 B> 01 001^b 01
  111 011 00 111^a 1101100 00 B> 001^b 01
  111 011 00 111^a 1101100 00 011^b B> 01
  111 011 00 111^a 1101100 00 011^b 01101 D> 0
  = 111 011 00 1^3a+2 011 D(4, 0, b+1)
  111 011 00 1^3a+2 011 D(3, 3b+5, 0)

A(a, 2k+1)
  111 011 00 1^3a+2 011 D(3, 6k+8, 0)
  111 011 00 1^3a+2 011 D(0, (6k+9) 3^3 - 1, 0)
  = 111 011 00 1^3a+2 D(1, (6k+9) 3^3 + 1, 0)
  111 011 00 1^3a+2 D(0, (6k+9) 3^4 + 5, 0)
  = 111 011 00 1^3a+2 D(0, (6k+9) 3^4 + 5, 0)



Rule 1:  D(a, b+3, c) -> D(a, b+1, c+2)
Rule 1x: D(a, 2k+r, c) -> D(a, r, c+2k)

Rule 2: D(a+1, 0, c) -> D(a, 3c+2, 0)

Rule 3: D(a+1, 2k) -> D(a+1, 0, 2k) -> D(a, 6k+2) = D(a, 3b+2)
Rule 3x: D(a, 2k) -> D(0, (2k+1) 3^a - 1)

Rule 4: D(4, 1, c+1) -> 111 011 D(2, 3c+1, 0)
Rule 5: 1 D(2, 1, c) -> 1011 D(4, 0, c) -> 1011 D(3, 3c+2, 0)



0000 1011 <A  -> 11101100 B>
  000010 <D 00
  0000 <E 0100
  00 <D 001010
  11101100 B>

B> 100 -> 111 B>
(11) B> 1010 -> 101100 B>
B> 01 -> 00 B>
B> 001 -> 011 B>

B> 01 0000 -> <A 100101
  0001 D> 00
  01101 D> 0
  <A 100 101
```

```
       +-----+-----+
       |  0  |  1  |
   +---+-----+-----+
   | A | 1LE | 0LD |
   +---+-----+-----+
   | B | 0RC | 0RE |
   +---+-----+-----+
   | C | 0RD | 1RC |
   +---+-----+-----+
   | D | 1LA | 1LF |
   +---+-----+-----+
   | E | 1RB | 0LA |
   +---+-----+-----+
   | F | 1LD | 1RZ |
   +---+-----+-----+


Rule 1:  00 <E 10^n 11 -> <E 10^n+2
Rule 1x: 0^inf <E 10^n 11^m -> 0^inf <E 10^n+2m

Rule 2:  0000 <E 10^n 0101 -> <E 1^2n+8 -> 0^inf <E 10^2n+8
Rule 2x: 0^inf <E 10^n 0101^m -> 0^inf <E 10^((n+8) 2^m - 8)

Rule 3:  000 <E 10^n 000 -> <E 11 0 10^n 010 -> <E 10^2 01^n 00 10
Rule 4:  00 <E 10^n 0100 -> <E 10^n+2 11 -> 0^inf <E 10^n+4



@64:  <E 10^7
R 3:  <E 10^2 01^7 00 10
R 2x: <E 10^(10 2^3 - 8) 0100 10   # = 72
R 4:  <E 10^76 10
R 3:  <E 10^2 01^77 0010



Repeat forever:
        <E 10^2k+1
  R 3:  <E 10^2 01^2k+1 0010
  R 2x: <E 10^(10 2^k - 8) 0100 10
  R 4:  <E 10^(10 2^k - 4) 10  =  <E 10^(10 2^k - 3)
```


### 5x2 Qhalt

This is the latest Quasihalting TM I've found in 5x2 with at least 1 undefined transition (i.e. it's part of the BB(5, 2) search).

`1RB---_0LC0LB_0LD1LC_1RD0RE_1LB1LA`

```
D(a, b, c) = 0^inf 1^a D> 1^b 0 1^c 0^inf

D(a, b+2, c) -> D(a+5, b, c)
D(a, 2, 0) -> Qhalt
D(a, 2, c) -> D(a+6, c, 0)
D(a, 1, c) -> D(2, a, c+1)

0 : D(2, 1, 0)   @ step 6
1 : D(2, 2, 1)
2 : D(8, 1, 0)
3 : D(2, 8, 1)
4 : D(17, 2, 1)
5 : D(23, 1, 0)
6 : D(2, 23, 1)
7 : D(57, 1, 1)
8 : D(2, 57, 2)
9 : D(142, 1, 2)
10 : D(2, 142, 3)
11 : D(352, 2, 3)
12 : D(358, 3, 0)
13 : D(363, 1, 0)
14 : D(2, 363, 1)
15 : D(907, 1, 1)
16 : D(2, 907, 2)
17 : D(2267, 1, 2)
18 : D(2, 2267, 3)
19 : D(5667, 1, 3)
20 : D(2, 5667, 4)
21 : D(14167, 1, 4)
22 : D(2, 14167, 5)
23 : D(35417, 1, 5)
24 : D(2, 35417, 6)
25 : D(88542, 1, 6)
26 : D(2, 88542, 7)
27 : D(221352, 2, 7)
28 : D(221358, 7, 0)
29 : D(221373, 1, 0)
30 : D(2, 221373, 1)
31 : D(553432, 1, 1)
32 : D(2, 553432, 2)
33 : D(1383577, 2, 2)
34 : D(1383583, 2, 0)
35 : Qhalt
```


## 6x2 Level 3 Infinite

https://bbchallenge.org/1RB1LA_1RC0LE_1RD1RB_0LA0LF_1LC1LB_---1LD

A TM Pavel shared on Discord which does Level 3 Collatz recurrence (like https://www.sligocki.com/2022/06/21/bb-6-2-t15.html) but none of the Collatz cases halt:

```
Let A(a, b, c) = 0^inf <A 01^a 00 10^b 1^c 0^inf

Level 1:
  A(a, b+1, c) -> A(a+2, b, c)
  A(a, 0, c+5) -> A(1, a+5, c)
  A(a, 0, 0) -> A(1, 2, 2a)
  A(a, 0, 1) -> A(1, 2, 2a+4)
  A(a, 0, 2) -> A(1, 2, 2a+6)
  A(a, 0, 3) -> A(1, 2, 2a+8)
  A(a, 0, 4) -> A(1, 2, 2a+10)

Level 2:
  A(a, 0, c+5) -> A(2a+11, 0, c)

Level 3:
  A(1, 2, 5k  ) -> A(1, 2, 32 2^k - 22)
  A(1, 2, 5k+1) -> A(1, 2, 32 2^k - 18)
  A(1, 2, 5k+2) -> A(1, 2, 32 2^k - 16)
  A(1, 2, 5k+3) -> A(1, 2, 32 2^k - 14)
  A(1, 2, 5k+4) -> A(1, 2, 32 2^k - 12)
```

Level 1 rules are actually all you need. The set $$\{A(a, b, c) : a, b, c \in \mathbb{N}\}$$ is closed, so once TM enters it cannot halt.



## 6x2 Level 3 Tricky Proof

https://bbchallenge.org/1RB0LF_1LC1RD_1LA0RC_0RE1RB_0LC1LA_---1LE

Another TM Pavel shared on Discord which also has Level 3 Collatz recurrence and some of the Collatz cases even halt! But it skillfully avoids them:

```
Let C(a, b, c) = $ 1100 (01111 1100)^a 1100 0110^b 1100^c C> 1 $

@93: C(0, 0, 1)

Level 1:
  C(a, b+1, c) -> C(a, b, c+2)
  C(a+1, 0, c) -> C(a, c+2, 2)

  C(0, 0, 9k+1) -> C(4k+1, 0, 2)
  C(0, 0, 9k+4) -> C(4k+2, 1, 2)
  C(0, 0, 9k+7) -> C(4k+4, 0, 1)

  C(0, 0, 3k+2) -> Halt

Level 2:
  C(a+1, 0, c) -> C(a, 0, 2c+6)
  *: C(a, 0, c) -> C(0, 0, (c+6) 2^a - 6)

Level 3:
  C(0, 0, 9k+1) -> C(0, 0,  8 2^{4k+1} - 6)
  C(0, 0, 9k+4) -> C(0, 0, 10 2^{4k+2} - 6)
  C(0, 0, 9k+7) -> C(0, 0,  7 2^{4k+4} - 6)
```

The trick is that we can rewrite the Level 3 rules as:
```
  C(0, 0, 9k+1) -> C(0, 0,    4^{2k+2} - 6)
  C(0, 0, 9k+4) -> C(0, 0, 10 4^{2k+1} - 6)
  C(0, 0, 9k+7) -> C(0, 0,  7 4^{2k+2} - 6)
```

And since $$4^n \equiv 1 \mod 3$$ (and so do 10 and 7), the right sides will always be remainder 1 (mod 3) and so always one of remainders (1, 4, 7) mod 9.

But it is delicate! A slight tweak, like `7 2^{4k+1} - 6` and this condition would not hold any longer!

All of the permutations of this TM fall into the same starting config `C(0, 0, 1)` except for starting from state B in which case we reach `C(0, 0, 0)` which leads to the rule:

```
C(0, 0, 9k+0) -> C(0, 0, 10 2^{4k+1} - 7) = C(0, 0, 5 4^{2k+1} - 7)
```

But that is also guaranteed to always be remainder 1 (mod 3)! and so will never halt.
