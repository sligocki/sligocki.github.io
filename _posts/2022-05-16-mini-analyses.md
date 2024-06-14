---
layout: post-custom
title: Mini Analyses
tags: busy-beaver collatz
---

I spend a lot of time doing "quick-and-dirty" analyses of new Busy Beaver champions. Recently, when I have time, I have been posting those analyses on this blog in detailed form. However, converting from my quick notes to a polished blog post is a lot of time and work and so most of my analyses don't make it. However, I think there is some value in the quick notes, so I keep them in unsaved documents in my editor until I end up with too many tabs (or my computer crashes) and eventually I delete them. As a middle ground, I've decided to try and post unpolished notes here as a value to myself if I ever come back and want to understand these machines behavior again in the future. I may add more notes here over time.


## 2022
### May 2022

#### Pavel-e197k

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


#### Machine Adjacent to Pavel-e197k

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


#### Machine Adjacent to e78k

While analyzing 6x2 machines "adjacent" to the short-lived 6x2 champion I announced on Friday (which runs for >10^78913 steps), I found exactly one machine (with 3 different permutations of start state) which our filters aren't able to categorize. This is the same as my 6x2 former champion except with `A1 -> 1RA`.

`1RB 1RA  1LC 0RF  1RA 0LD  0LC 0LE  1LD 0RA  1RE 1RZ`

a0: `00001 <E 0101^a 1^3k   00  ->  1 <E 0101 1 0^4a 001^k 1`
a1: `  001 <E 0101^a 1^3k+1 00  ->  1 <E 0101^a+1 001^k 1`
a2: ` 0001 <E 0101^a 1^3k+2 00  ->  1 <E 0101 1^4a+2 001^k 1`

Once this machine gets into a tape matching regular expression `0^inf 1 <E (0101)+ (00|1)* 0^inf` it will never leave. Thus these machines are infinite.


#### Swapping Rules

`1RB 1RF  1LC 1LB  --- 0LD  1LE 1LF  1RE 1LD  0RF 0RA`

Rule 1:  `0100 01^a <D 01 11^b  ->  0100 01^b+1 <D 01 11^a`
Rule 1x: `0100 01^a <D 01 11^b  ->  0100 01^a+1 <D 01 11^b -> Infinite`


`1RB 1LC  0RC 1LE  1RD 0LA  1LB 1RC  1RC 1LF  --- 1LE`

Rule 1:  `01 11^a 10 C> 01^b 11 -> 01 11^b-2 10 C> 01^a+4 11`
Rule 1x: `01 11^a 10 C> 01^b 11 -> 01 11^a+2 10 C> 01^b+2 11 -> Infinite`


#### Three Stage Rules

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


### June 2022

#### Halting 6x2

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


### Oct 2022

#### 5x2 Qhalt

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


## 2023
### March 2023

#### 6x2 Level 3 Infinite

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



#### 6x2 Level 3 Avoids Halt

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

A re-analysis which makes absorbs these coincidences is:

```
E(a, b, c) = C(2a, 3b, 3c+1) = $ 1100 {01111 1100 01111 1100}^a 1100 {0110 0110 0110}^b 1100 {1100 1100 1100}^c C> 1 $

E(a, b+1, c) -> E(a, b, c+2)
E(a+1, 0, c) -> E(a, 0, 4c+7)
E(0, 0, 3k)   -> E(2k  , 0, 3)
E(0, 0, 3k+1) -> E(2k+1, 0, 1)
E(0, 0, 3k+2) -> E(2k+2, 0, 0)

And Blank -> E(0, 0, 0) in 93 steps.
```

At which point we have a closed set, so presumably this TM is solveable by CTL with regular expression: `1100 (01111 1100 01111 1100)* 1100 (0110 0110 0110)* 1100 (1100 1100 1100)* C> 1`?


#### 6x2 Another Avoids Halt

https://bbchallenge.org/1RB1RD_1LC1LF_0LD1LB_1RE0RA_1RA1RE_---1LD

TM shared by @savask on Discord: https://discord.com/channels/960643023006490684/1026577255754903572/1091130641317372035

```
1RB1RD_1LC1LF_0LD1LB_1RE0RA_1RA1RE_---1LD

C(a, b, c, d) = $ 1^a 10^b 1^c 01^d <C 1 $

Level 1:
  C(a, b, c+4, d) -> C(a, b, c, d+3)

  C(a, b, 2, d) ->  HALT!

  C(a, b+1, 0, d) -> C(a, b, 2d+5, 0)
  C(a, b+1, 1, d) = C(0, 0, a+1, b+d+1)
  C(a, b+1, 3, d) -> C(a, b, 2d+8, 0)

  C(0, 0, 0, d) -> C(2, d, 4, 0)
  C(0, 0, 1, d) = C(0, 0, 0, d+1)
  C(0, 0, 3, d) -> C(0, 0, 2d+7, 0)

Level 2:
  C(a, b+1, 4k+0, 0) -> C(a, b, 6k+5, 0)
  C(a, b+1, 4k+1, 0) -> C(0, 0, a+1, 3k+b+1)
  C(a, b+1, 4k+2, 0) ->  HALT!
  C(a, b+1, 4k+3, 0) -> C(a, b, 6k+8, 0)

Level 3:
  C(0, 0, 4k+1, 0) -> C(0, 0, 6k+131, 0)     for k >= 2
  C(0, 0, 4k+3, 0) -> C(0, 0, 6k+7, 0)
```

So, you can see the "Level 3" rules are closed, but the way that `C(0, 0, 4k+1, 0) -> C(0, 0, 6k+131, 0)` is proven is wild! Basically it repeats the same sequence of "Level 2" rules each time (and they happen to never hit the `4k+2` case):

```
Proof of C(0, 0, 4k+1, 0) -> C(0, 0, 6k+131, 0):
  C(0, 0, 4k+1, 0) -> C(0, 0, 1, 3k) = C(0, 0, 0, 3k+1)
                   -> C(2, 3k+1,  4, 0)
                   -> C(2, 3k+0, 11, 0)
                   -> C(2, 3k-1, 20, 0)
                   -> C(2, 3k-2, 35, 0)
                   -> C(2, 3k-3, 56, 0)
                   -> C(2, 3k-4, 89, 0)
                   -> C(0, 0, 3, 3k+62)
                   -> C(0, 0, 6k+131, 0)
```


### April 2023
#### 3x3 Linear Rules
Among my 3x3 holdouts there are 8 that I can prove Linear Rules for:

```
Group A:
  1RB2RB1RC_1LC0LA---_1RA2LC1LB
  1RB0RC---_1LC2RB1RA_1LA2LA1LB
  1RB2LA1LC_1RC2RC1RA_1LA0LB---

Group B:
  1RB---2RA_0RC1RA0RB_2LC2LB0LA
  1RB2RA---_0RC2LB2RB_1LC0LA0RB
  1RB0RA---_0RC2LB2RB_1LC0LA0RB

Group C:
  1RB2LB2LA_1LA2RC1LB_---2RB0LB
  1RB1LA2LA_1LA2RC1LB_---2RB0LB
```

##### Group A

```
1RB2RB1RC_1LC0LA---_1RA2LC1LB

$ 1 11^a A> 22 _    -> $ 1 11^a+2 A> _
$ 1 11^a A> 21 _    -> $ 1 A> 22^a+1 2 _
$ 1 11^a A> 2000 _  -> $ 1 A> 22^a+2 1 _
$ 1 11^a A> 10 _    -> $ 1 A> 2 1^2a+1 _

A(a, b, c) = $ 1 11^a C> 2^b 1^c $

Level 1:
  A(a, b+2, c)    -> A(a+2, b, c)
  A(a, 1,   c+1)  -> A(0, 2a+3, c)
  A(a, 1,   0)    -> A(0, 2a+4, 1)
  A(a, 0,   1)    -> A(0, 1, 2a+1)

Level 2:
  A(a, 2k+r, c) -> A(a+2k, r, c)
  A(0, 2k+1, c+1) -> A(0, 4k+3, c)   4k+3 = 2 (2k+1) + 1
  A(0, 2k, 1) -> A(0, 1, 4k+1)

Level 3:
  A(0, 1, c) -> A(0, 2^c+1 - 1, 0)
             -> A(2^c+1 - 2, 1, 0)
             -> A(0, 2^c+2, 1)
             -> A(0, 1, 2^c+3 + 1)   [Infinite Rule]

Starts in A(0, 1, 1) @4
```

The next two are just permutations of the first with start configs:

```
1RB0RC---_1LC2RB1RA_1LA2LA1LB  :  Starts in A(0, 1, 3) @10
1RB2LA1LC_1RC2RC1RA_1LA0LB---  :  Starts in A(0, 1, 7) @38
```

##### Group B

These are not permutations of each other, but do share a lot of rules

```
1RB---2RA_0RC1RA0RB_2LC2LB0LA

  _ 22 0^c C> $ -> _ 0^c+6 C> $
  _ 01 0^c C> $ -> _ 1^c+2 0 C> $
  _ 11 0^c C> $ -> _ 1 2^c+1 0^5 C> $

D(a, b, c) = $ 1^a+1 2^2b 0^2c+1 C> $

Level 1 (Closed Set):
  D(a, b+1, c) -> D(a, b, c+3)
  D(a+1, 0, c) -> D(a, c+1, 2)
  D(0, 0, c) -> D(2c+2, 0, 0)

Level 2:
  D(a, b, c) -> D(a, 0, c+3b)
  D(a+1, 0, c) -> D(a, 0, 3c+5)
  D(a, 0, c) -> D(0, 0, (c+5/2) 3^a - 5/2)

Level 3:
  D(0, 0, c) -> D(0, 0, 5/2 (3^2c+2 - 1))   [Infinite Rule]

Starts in D(0, 0, 0) @2
```

Next: 1RB2RA---_0RC2LB2RB_1LC0LA0RB

```
1RB2RA---_0RC2LB2RB_1LC0LA0RB

  _  2 0^c C> 00 _ -> _ 0^c+3 C> _
  _ 01 0^c C> 0  _ -> _ 1^c+2 0 C> _
  _ 11 0^c C> 00 _ -> _ 2^c+3 0 C> _

C(a, b, c) = $ 1^a 2^b 0^c C> $

Level 1 (Closed Set):
  C(a,   b+1, c)  ->  C(a,   b,   c+3)
  C(a+2, 0,   c)  ->  C(a,   c+3, 1)
  C(1,   0,   c)  ->  C(c+2, 0,   1)
  C(0,   0,   c)  ->  Inf Translated Cycle

Starts in C(1, 0, 1) @2
```

Note that we can even prove that this TM never reaches the `C(0, 0, c)` transition:
```
Level 2:
  C(a,   b, c)  ->  C(a, 0, c+3b)
  C(a+2, 0, c)  ->  C(a, 0, 3c+10)

Level 3:
  C(2k+1, 0, 1) -> C(1, 0, 6 3^k - 5)
                -> C(6 3^k - 3, 0, 1)   [Infinite Rule]
```


Next: 1RB0RA---_0RC2LB2RB_1LC0LA0RB is very similar to the previous one.

```
1RB0RA---_0RC2LB2RB_1LC0LA0RB

  _  2 0^c C> 00 _ -> _ 0^c+3 C> _
  _ 01 0^c C> 0  _ -> _ 1^c+2 0 C> _
  _ 11 0^c C> 00 _ -> _ 00 2^c+1 0 C> _

C(a, b, c) = $ 1^a 00 2^b 0^c C> $

Level 1 (Closed Set):
  C(a,   b+1, c)  ->  C(a,   b,   c+3)
  C(a+2, 0,   c)  ->  C(a,   c+3, 1)
  C(1,   0,   c)  ->  C(c+2, 2,   1)
  C(0,   0,   c)  ->  Inf Translated Cycle

Starts in C(1, 2, 1) @27
```

Like the last TM we have rules:
```
Level 2:
  C(a, b, c)  ->  C(a, 0, c+3b)
  C(a+2, 0, c)  ->  C(a, 0, 3c+10)

Level 3:
  C(2k+1, 2, 1) -> C(2k+1, 0, 7)
                -> C(1, 0, 12 3^k - 5)
                -> C(12 3^k - 3, 2, 1)   [Infinite Rule]
```

##### Group C

```
1RB2LB2LA_1LA2RC1LB_---2RB0LB

  _  00 <A 11^n 22 _ -> _ <A 11^n+2 _
  _   0 <A 11^n 12 _ -> _ <A 22^n+1 1 _
  _ 000 <A 11^n 0  _ -> _ <A 22 1 2^2n 1 _

A(a, b, c) = $ <A 11^a 22^b 1 2^c 1 $

Level 1 (Closed Set):
  A(a, b+1, c)    ->  A(a+2, b,   c)
  A(a, 0,   c+1)  ->  A(0,   a+1, c)
  A(a, 0,   0)    ->  A(0,   1,   2a+2)

Level 2:
  A(a, b, c)  ->  A(a+2b, 0, c)
  A(a, 0, c+1) -> A(2a+2, 0, c)

Level 3:
  A(a, 0, c) -> A((a+2) 2^c - 2, 0, 0)
             -> A(0, 1, (a+2) 2^c+1 - 2)
             -> A(2, 0, (a+2) 2^c+1 - 2)   [Infinite Rule]

Starts at A(0, 0, 1) @3
```

And the last TM is extremely similar, but with a small tweak to the `A(a, 0, 0)` rule.

```
1RB1LA2LA_1LA2RC1LB_---2RB0LB

  _  00 <A 11^n  22 _ -> _ <A 11^n+2 _
  _   0 <A 11^n  12 _ -> _ <A 22^n+1 1 _
  _  00 <A 11^n+1 0 _ -> _ <A 22 1 2^2n+1 1 _

A(a, b, c) = $ <A 11^a 22^b 1 2^c 1 $

Level 1 (Closed Set):
  A(a, b+1, c)    ->  A(a+2, b,   c)
  A(a, 0,   c+1)  ->  A(0,   a+1, c)
  A(a, 0,   0)    ->  A(0,   1,   2a+1)

Level 2:
  A(a, b, c)  ->  A(a+2b, 0, c)
  A(a, 0, c+1) -> A(2a+2, 0, c)

Level 3:
  A(a, 0, c) -> A((a+2) 2^c - 2, 0, 0)
             -> A(0, 1, (a+2) 2^c+1 - 3)
             -> A(2, 0, (a+2) 2^c+1 - 3)   [Infinite Rule]

Starts at A(0, 0, 0) @3
```

#### 6x2 Mixed Exponential and Add

```
1RB0RD_0RC0RA_1LD0LC_1LE1LB_0RE0LF_1LA---

  $ 01^a  B> 0110 -> $ 01^a+4 B>
  $ 01^2a+2 B> 00 -> $ 01^3 B> 0110^a 011
  $ 01^2a+1 B> 00 -> $ 01^2 B> 110 0110^a 011

B(a, b, c) = $ 0101^a B> 110 10^b+1 0110^d $
C()

Closed Set:
  B(a, b, c+1) -> B(4a+5, b+1, c-1)
  B(a, b, 0)   -> B(7, 0, a+2b+2)

Starts in B(7, 0, 3) @405
```

This proof (by closed set) is complete, but if you try to evaluate the trajectory here, it's slightly more interesting:

```
B(7, 0, c) -> B((7 + 5/3) 4^c - 5/3, c, 0)
           -> B(7, 0, (26 4^c + 1)/3 + 2c)
```

So we are recursing the map `c -> (26 4^c + 1)/3 + 2c` which has both exponential and addition.


#### 6x2 ExpInt close

```
1RB0RC_1LC1RA_1RE1RD_1LF0RB_---1LA_1LB0LD

Rules:
  00^inf 10 11^f 10^g 11^h-1 01^i+2 (01) D> 00^inf
  00^inf 10 11^(j - 1) 10^(k + 2) 11^1 01^(2 l + 4) (01) D> 00^inf


       11 01^c D> $ -> 01^c+2 D> $
  11 10^b 01^c D> $ -> 10^b+2 11^c 01 D> $
    11 01^a A> 10^2 -> 01^a+3 A>


D(a, b, c, d) = $ 10 11^a 10^b 11^c 01^d D> $

Level 1:
  D(a, b, c+1, d) -> D(a, b, c, d+2)
  D(a, b, 0, d) -> D(a-1, b+2, d, 1)
  D(0, b, 0, d) -> D(3b+5, 3, d-2b-5, 1)   (Assuming d > 2b+5)

Level 2:
  D(a, b, c, d) -> D(a, b, 0, d+2c)
  D(a, b, 0, d) -> D(a-1, b+2, 0, 2d+1)
  D(0, b, 0, d) -> D(3b+5, 3, 0, 2d-4b-9)

Level 3:
  D(a, b, 0, d) -> D(0, b+2a, 0, (d+1) 2^a - 1)
  D(0, b, 0, d) -> D(0, 6b+13, 0, (d-2b-4) 2^(3b+6) - 1)   [Infinite rule]

Starts at D(0, 0, 3, 1) @35
```

The last rule is infinite (b/c $$d > b$$ remains true forever), but the sequence produces progressively more and more complicated terms:

```
D(0, 0, 0, 7)
D(0, 13, 0, 3 2^6 - 1)
D(0, 7*13, 0, (3 2^6 - 31) 2^45 - 1)
D(0, 13(1+6+6^2), 0, ((3 2^6 - 31) 2^45 - 187) 2^279 - 1)
```


### May 2023
#### Pavel's BB(2, 6) > 10^^91

`1RB3LA4LB0RB1RA3LA_2LA2RA4LA1RA5RB1RZ`

This is the current 2x6 champion found by Pavel last month. Like my 10^^70 TM, it seems to have a mix of orderly and chaotic behavior. (As usual, what I am calling chaotic is really just behavior I haven't been able to make sense of, maybe someone else will be able to make an orderly analysis at some point.)

The orderly behavior is built upon these two rules:
```
01 11 0211^n B> 00  ->  11 0211^n+1 B>
  Repeated: 01^k 11 0211^n B> $  ->  11 0211^n+k B> $

01^3 11 01 21 0211^n B> $ -> 11 01 21 0211^4n+10 B> $
  Repeated: 01^3k+r 11 01 21 0211^n B> $  ->  01^r 11 01 21 0211^{((3n+10) 4^k - 10)/3} B> $
```

Most notably, a single simplification of the last rule basically accounts for all of the 10^^91 growth:

```
01^3k+r 11 01 21 0211^17 B> $  ->  01^r 11 01 21 0211^{(61 4^k - 10)/3} B> $
```

Which is applied ~90 times thus leading to ~4^^90 score.

Other rules I discovered while analyzing:

```
Hand Rules:
  01 11 0211^n B> 00  ->  11 0211^n+1 B>

                    01 0211^n B> 00    ->        12 11 0211^n   B>
                 12 11 0211^n B> 00    ->           21 0211^n+1 B>
              12 11 11 0211^n B> 00    ->        21 01 0211^n+1 B>
        01 11 02 11 11 0211^n B> 00    ->  11 02 11 01 0211^n+1 B>
      01 11^a 02 21 11 0211^n B> 0000  ->    11 01^a+1 0211^n+2 B>
   01 1551 11 02 11 11 0211^n B> 00    -> 11 01^2 02 11 01 0211^n+1 B>

       01 11^a+1 0211^n B> 00    ->            11 01^a  0211^n+1 B>
         0 11 11 0211^n B> 000   ->         11 11 01^2n 0211     B>
         1 21 11 0211^n B> 000   ->         21 11 01^2n 0211     B>
  0 11^a|1 01 21 0211^n B> 000   ->  11 01^a-1 1551 01^2n 0211     B>


         01 01 21 0211^n B> 0000  ->     12 11 01^2n 11 0211 B>
      01 11 01 21 0211^n B> 0000  ->  11 02 11 01^2n 11 0211 B>
         01 21 21 0211^n B> 0000  ->     11 11 01^2n 11 0211 B>
            11 21 0211^n B> 0000  ->        21 01^2n 11 0211 B>

Simplified:
  01 11   02 11 11 0211^n B> $  -> 11 02 21 11 0211^2n+5 B> $
  01 11^a 02 21 11 0211^n B> $  -> 11 01^a  21 0211^n+3  B> $
  01 1551 11 02 11 11 0211^n B> 00 -> 11 01 21 11 0211^4n+17 B> $

         01 11^a+2 0211^n B> $  ->  11 01^a 21 0211^n+2  B> $
         0 11 11 0211^n|3 B> $  ->           11 11  01^2n-5 11 01 21 0211^17 B> $
         1 21 11 0211^n|3 B> $  ->           21 11  01^2n-5 11 01 21 0211^17 B> $
  0 11^a|1 01 21 0211^n|3 B> $  ->  11 01^a-1 1551  01^2n-5 11 01 21 0211^17 B> $

     01 01 21 0211^n   B> $  ->        21 21 0211^2n+3 B> $
  01 11 01 21 0211^n   B> $  ->  11 02 11 11 0211^2n+1 B> $
     01 21 21 0211^n   B> $  ->     11 11 11 0211^2n+1 B> $
        11 21 0211^n   B> $  ->        21 11 0211^2n+1 B> $

      01^3 21 0211^n   B> $  ->   11 11 11 0211^4n+7 B> $
      01^4 21 0211^n   B> $  ->   11 01 21 0211^4n+9 B> $

      01^3 11 01 21 0211^n B> $ -> 11 01 21 0211^4n+10 B> $
      01^3k 11 01 21 0211^n B> $ -> 11 01 21 0211^{(n+10/3) 4^k - 10/3} B> $


Start:
  21 0211 B>
  11 01^3 11 0211 B>
  11 11 0211^4 B>
  11 11 01^3 11 01 21 0211^17 B>
  11 11      11 01 21 0211^78 B>  [@q279]
  11 01^2 1551  01^151 11 01 21 0211^17 B>  [@q390+s5]
    151 = 3 (50) + 1
    a = (61 4^50 - 10)/3
  11 01^2 1551 01 11 01 21 0211^a  B>
  11 01^2 1551 11 02 11 11 0211^2a+1 B>
  11 01 11 01 21 11 0211^8a+21 B>
  11 01 11 0 21 11  01^16a+37 11 01 21 0211^17 B>
    4^3 % 9 = 1  52 % 3 = 1  4^52 % 9 = 4^1
    61 4^52 - 49 % 9 = 7 4 - 4 % 9 = 24 % 9 = 6
    16a+37 = (61 4^52 - 49)/3 = 3 k_a + 2
    b = (61 4^k_a - 10)/3
  11 01 11 0 21 11 01^2 11 01 21 0211^b  B>

```

But this process doesn't show any sign of simplifying over time. There may be endlessly many rules like this ...

## 2024
### June 2024
#### Coincidence Collatz

`1RB1RD_0LC0RD_1LD1LC_1RA1LE_0RE0RF_1RZ0LC`

Longest running halter of set of 9 halting TMs shared by @mxdys on 8 June 2024. <https://discord.com/channels/960643023006490684/1026577255754903572/1249137796778823760>

This took Quick_Sim.py 12 hours and almost 5 billion simulator steps to solve!

```
1RB1RD_0LC0RD_1LD1LC_1RA1LE_0RE0RF_---0LC

A(a, b, c) = 0^inf 11^a 01 11^b A> 1^c 0^inf

A(a, b+1, c+2)  -->  A(a+2,   b,    c)

A(a,   b,   0)  -->  A(  0,   a, 2b+3)
A(a,   b,   1)  -->  A(  a, b+1,    0)
A(a,   0, c+7)  -->  A(  2, a+2,    c)

A(a,   0,   2)  -->  A(  0, a+1,    0)
A(a,   0,   3)  -->  A(a+2,   0,    0)
A(a,   0,   4)  -->   Halt(2a+5)  $ 11^a+2 00 1 Z> $
A(a,   0,   5)  -->  A(  0, a+3,    0)
A(a,   0,   6)  -->  A(  0, a+4,    0)

@61: A(0, 0, 9) = $ 1 A> 1^9 $
```

The trick here is that this TM reduces both `b` and `c` at the same time. If one runs out enough before the other, then it continues, but only when the both "run out" at the same time (`b = 0` and `2 <= c < 7`) does it have a chance of halting. And then only 1 of 5 remainders leads to halt. Furthermore it gets very lucky and doesn't hit that 1 of 5 remainders until the 18th try!

All iterations where `b = 0` and `2 <= c < 7`:

```
         36  A(         22,           0,           6)
         61  A(         38,           0,           2)
        100  A(         56,           0,           3)
        431  A(        230,           0,           2)
        709  A(        368,           0,           5)
      4_115  A(      2_058,           0,           6)
      5_987  A(      2_994,           0,           3)
      6_060  A(      3_028,           0,           3)
     16_028  A(      7_950,           0,           6)
     27_428  A(     13_652,           0,           3)
    322_000  A(    160_934,           0,           2)
    944_198  A(    472_128,           0,           5)
  1_778_129  A(    889_264,           0,           5)
  5_695_317  A(  2_847_234,           0,           5)
178_941_119  A( 89_468_770,           0,           5)
305_476_095  A(152_733_912,           0,           3)
585_453_777  A(292_712_846,           0,           4)
Halted: Score: 585_425_697
```

Final config:

```
00^inf 01^1 11^292_712_847 10^1 (01) Z> 00^inf
```

#### BB(6, 2, -1) Step Champ

AFAICT this is the current BB6 (with one unfilled transition) step champion: https://bbchallenge.org/1RB1RA_1LC1RF_1RE1LD_0RD1LB_---0RA_1RZ0LE

```
1RB1RA_1LC1RF_1RE1LD_0RD1LB_---0RA_1RZ0LE

Steps:    ~10^13.39223  =  24_673_582_891_560
Nonzeros: 2_323_223  =  2_323_223

C(a, b, c) = $ 1^a 0 1^b <C 1^2c+1 $

C(a, b+3, c) -> C(a, b, c+2)

C(a, 0, c) -> C(a+1, 2c+1, 0)
C(a, 1, c) -> C(0, a, c+1)

C(0, 2, c) -> C(1, 2c+4, 0)
C(1, 2, c) -> C(1, 2c+6, 0)
C(2, 2, c) -> C(1, 2c+7, 0)
C(a+3, 2, c) -> 1^a 01^c+3 11 Z>

@2: C(0, 1, 0)
```

It does one of these interesting Collatz Markov Chains where it only halts if `a>=3` and `b%3 = 2` and otherwise has a couple ways to reset `a <= 1`. But eventually after flipping that coin 62 times it finally halts.

Many of the other top BB(6, 2, -1) TMs seem to behave similarly (at least judging by the identical scores!):

```
grep -- --- Machines/bb/6x2.txt
1RB0RC_1LC1LF_1RD0LB_1RZ0LE_---1RA_1LB0RE Halt ~10^13.2 4_059_761
1RB0RC_1LC1LE_1RD0LB_1RZ1RB_1LB0RF_---1RA Halt ~10^13.2 4_059_761
1RB1RA_1LC1RF_1RE1LD_1LC1LB_---0RA_1RZ0LE Halt ~10^13.3 2_323_223
1RB1RA_1LC1RF_1RE1LD_0RD1LB_---0RA_1RZ0LE Halt ~10^13.4 2_323_223
1RB1RE_1LC1RA_---0LD_1LE1LD_1RB1LF_1RZ0RC Halt ~10^13.3 2_323_223
1RB1LF_1LC1RE_---0LD_1LA1LD_1RB1RA_1RZ0RC Halt ~10^13.3 2_323_223
1RB1LF_1LC1RE_---0LD_1LA1LD_0LE1RA_1RZ0RC Halt ~10^13.4 2_323_223
1RB1RA_1LC0RF_1RE1LD_1LC1LB_---0RA_1RZ1RB Halt ~10^13.1 2_323_222
1RB0LF_1LC1RE_---0LD_1LE1LD_1RB1RA_1RZ1LA Halt ~10^13.1 2_323_222
1RB1RA_1LC0RF_1RE1LD_0RD1LB_---0RA_1RZ1RB Halt ~10^13.3 2_323_222
1RB1RE_1LC1RA_---0LD_1LE1LD_1RB0LF_1RZ1LE Halt ~10^13.1 2_323_222
1RB1RA_1LC1LE_1RD1LB_---0RA_1LC0RF_1RZ1RE Halt ~10^13.1 2_323_222
1RB0LF_1LC1RE_---0LD_1LA1LD_1RB1RA_1RZ1LA Halt ~10^13.1 2_323_222
1RB1RE_1LC1RA_---0LD_1LA1LD_1RB0LF_1RZ1LE Halt ~10^13.1 2_323_222
1RB0LF_1LC1RE_---0LD_1LA1LD_0LE1RA_1RZ1LA Halt ~10^13.3 2_323_222
```