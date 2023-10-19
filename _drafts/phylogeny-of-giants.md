---
layout: post-custom
title: Phylogeny of Giants
tags: busy-beaver
---

## Proven Quasihalting

 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  0RC 0LA
 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  0LD 0LA
10^14_006 steps

 * 1RB 1LE  0LC 0LB  0LE 1LC  1RD 1RA  1RD 0LA  (aka: 1RB 1LD  0LC 0LB  0LD 1LC  1RE 0LA  1RE 1RA)
 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  1RD 0LA
10^12_978 steps

 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  0RD 0LA
10^4079 steps

## "Probviously" quasihalting

GD(n, m) = 0^inf <D 0^n 1^m 0^inf
GC(n, m) = 0^inf <C 0^n 1^m 0^inf

 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  1RC 0LA
 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  1LC 0LA
 * 1LC 0LE  0LC 0LB  0LD 1LC  1RD 1RE  1RB 1LA  (A<->E from above. Identical behavior after step 2)
... -> GD(1, 10^286_575) -> ?

 * 1RB 1LE  0LC 0LB  1RD 1LC  1RD 1RA  0RC 0LA
 * 1RB 1LE  0LC 0LB  1RD 1LC  1RD 1RA  1RD 0LA
... -> GC(0, 10^146_129.8) -> ?

 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  1LA 0LA
... -> GD(1, 10^25_886) -> ?

 * 1RB 1LE  0LC 0LB  0LD 1LC  1RD 1RA  0LC 0LA
... -> GD(1, 10^12_645) -> ?

 * 1RC 0LE  0LC 0LB  0LD 1LC  1RD 1RE  1RB 1LA  (E0->1RC and A<->E)
... -> GD(1, 10^62.63) -> ?

 * 1RB 1LE  0LC 0LB  1RD 1LC  1RD 1RA  1LB 0LA
... -> GC(0, 10^49.1) -> ?

 * 1RB 1LE  0LC 0LB  1RD 1LC  1RD 1RA  1LA 0LA
... -> GC(0, 10^19) -> ?

 * 1RE 0LE  0LC 0LB  1RD 1LC  1RD 1RE  1RB 1LA  (E0 -> 1RA and A<->E)
... -> GC(0, 10^16.78)

 * 1RB 1LE  0LC 0LB  1RD 1LC  1RD 1RA  0LB 0LA
... -> GC(0, 10^16.05) -> ?

## Not Quasihalting

 * 1RB 1LE  0LC 0LB  0LE 1LC  1RD 1RA  0LD 0LA  (aka: 1RB 1LD  0LC 0LB  0LD 1LC  0LE 0LA  1RE 1RA)
GD(4k+1, m+2) -> GD(8k+10, m+1)
GD(4k+2, m+2) -> GD(8k+ 9, m+1)
GD(n, 1) -> GD(2, n+3)


## Unknown
1RB 1LE  0LC 0RA  1RD 1LC  1RD 1RA  0LB 0LA
1RB 1LE  0LC 0RC  1RD 1LC  1RD 1RA  0LB 0LA
 * B1 -> 0R? from e16... machine
