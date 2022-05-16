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
