---
layout: post
title: Shift Overflow Counters
tags: busy-beaver skelet
---

In my [last post]({% post_url 2023-02-02-skelet-34 %}), I described the peculiar behavior of Skelet \#34. After sharing that post on the [bbchallenge.org Discord](https://discord.gg/3uqtPJA9Uv), Justin Blanchard and @Iijil shared some interesting machines with similar looking behavior. I will call these **Shift Overflow Counters**. They seem to characterized by having a completely orderly "Counter Phase" in which they implement basic double counter until one of the sides **overflows** (expands) at which point they **shift** the block offset leading to the other side counter needing to be "reparsed" (in Skelet \#34 this shifted `1000` -> `0100`).

## Known Examples

| Name | Machine | BBC # | Status |
| :-: | :-: | -: | :- |
| `sk15` | `1RB---_1RC1LB_1LD1RE_1LB0LD_1RA0RC` | [2,204,428](https://bbchallenge.org/2204428) | Infinite (Proof Sketch) |
| `sk26` | `1RB1LD_1RC0RB_1LA1RC_1LE0LA_1LC---` | [13,134,219](https://bbchallenge.org/13134219) | Infinite (Proof Sketch) |
| `sk33` | `1RB1LC_0RC0RB_1LD0LA_1LE---_1LA1RE` | [11,896,833](https://bbchallenge.org/11896833) | Infinite (Proof Sketch) |
| `sk34` | `1RB1LC_0RC0RB_1LD0LA_1LE---_1LA1RA` | [11,896,832](https://bbchallenge.org/11896832) | Infinite (Proven) |
| `sk35` | `1RB1LC_0RC0RB_1LD0LA_1LE---_1LA0LA` | [11,896,831](https://bbchallenge.org/11896831) | Infinite (Proven) |
| `h19k` | `1RB1LD_1RC0RB_1LA0LE_1LC0LA_1RZ1RB` | [Link](https://bbchallenge.org/1RB1LD_1RC0RB_1LA0LE_1LC0LA_1RZ1RB&s=30000) | Halt in 19283 steps |
| `h24k` | `1RB1LD_1RC0RB_1LA1LE_1LC0LA_1RZ0RD` | [Link](https://bbchallenge.org/1RB1LD_1RC0RB_1LA1LE_1LC0LA_1RZ0RD&s=30000) | Halt in 24567 steps |


## Machines alike Skelet \#34

All of the Skelet machines (`sk#`) have transition tables very closely related to that of Skelet \#34. I provide rough analysis below for all of these here:


### Skelet \#35 (`sk35`)

`sk35` has the exact same behavior as `sk34` (just slightly faster in one transition).

The only difference between these two machines is that the `E1` transition:
 * `sk34` has `E1 -> 1RA`
 * `sk35` has `E1 -> 0LA`

In fact, the only way to reach state `E` in either of these machines is via the path: `C0 -> 1LD`, `D0 -> 1LE`. So, to reach the `E1` transition, we must have tape config: `1 <E 11`. Simulating directly from here we get:
 * `sk34`: `1 <E 11 -> <A 011` in 3 step
 * `sk35`: `1 <E 11 -> <A 011` in 1 step


### Skelet \#33 (`sk33`)

`sk33` differs from `sk34` only in the `E1` transition as well. However, in this case, this leads to somewhat different behavior. For example, the "Counter Phase" behavior is now $$ D(n, m) \to D(n+1, m+2) $$ and the shift is to the left after overflow.

Some rules:
```
L(2k)   = L(k) 0000
L(2k+1) = L(k) 0001

G(n, m) = L(n)     <A 010 R(m)
D(n, m) = L(n) 000 <A 010 R(m)

G(n, m) -> G(n+1, m+1)  (if b(m) > 1)
  L(n) <A -> L(n+1) B>
    0001 <A -> <A 1111
    0000 <A -> 0001 B>
  E> R(n) -> <C R(n+1)
    E> 11 -> 11 E>
    E> 10 -> <C 11
    11 <C -> <C 10
  B> 010 -> 111 E>
  111 <C -> <A 010


G(2n, 2^k - 1) -> D(n, 2^k)
  0 <A 010 11^k 0 -> <A 010 10^k 11

D(n, m) -> D(n', 0, 2 m' + 1)  (if b(m) > 2n) (n' < n)
  1000 0000^k <A 010 R(m) -> 1001 0001^k <A 010 R(m + 2^{k+1} - 1)
    -> <A 010 11 10 R(m + 2^{k+1} - 1)


Start -> G(0, 0, 0, 13)  @ Step 83
```

Note: $$2 m^\prime + 1$$ is always odd, so each counter phase begins in a config $$G(0, 2m+1)$$ and so all end with $$n$$ even as well. The Reset Invariant $$b(m) > 2n$$ also seems to hold, but I have not thought deeply about why yet.


### Skelet \#26 (`sk26`)

`sk26` differs from `sk33` only in the `B0` transition. Specifically (using `sk33` state names):
 * `sk33`: `B0 -> 0RC`
 * `sk26`: `B0 -> 1RE`

Non-normalized: `1RB1LC_1RE0RB_1LD0LA_1LE---_1LA1RE`
TNF Normalized: `1RB1LD_1RC0RB_1LA1RC_1LE0LA_1LC---` (`C` -> `D` -> `E` -> `C`)

`sk26` appears to satisfy similar (although slightly more complex) rules to `sk34`. Specifically (Using the non-normalized state name):

```
L(2k)   = L(k) 0000
L(2k+1) = L(k) 0001

D(n, a, m) = L(n) 000 <C 101a R(m)
E(n, a, m) = L(n) 00  <C 101a R(m)
F(n, a, m) = L(n) 0   <C 101a R(m)
G(n, a, m) = L(n)     <C 101a R(m)

D(n, a, m) -> D(n+1, a, m+1)  (if b(m) > 1)
  L(n) 000 <C -> L(n+1) 000 B>
    1000 <C -> <C 1111
    0000 <C -> 1000 B>
  E> R(n) -> <C R(n+1)
    E> 11 -> 11 E>
    E> 10 -> <C 11
    11 <C -> <C 10
  B> 1010 -> 1111 E>
  1111 <C -> <C 1010
  B> 1011 -> 0111 E>
  0111 <C -> <C 1011


D(n-1, 0, 2^k - 1) -> E(n, 1, 2^k)
  0 1111 E> 11^k 0 -> <C 1011 10^k 11

E(n, a, m) -> E(n', 1, m')  (if b(m) > n) (n' < n)
  0100 0000^k <C 101a R(m) -> 0100 1000^k <C 101a R(m + 2^k - 1)
  0100 1000^k <C 101a R(m + 2^k - 1) -> <C 1011 10^2k 11 1a R(m + 2^k - 1)


D(n-1, 1, 2^k - 1) -> G(2n, 0, 2^{k-1})
 1000 0000^n 0111 E> 11^m+1 0 -> 01 0000^n+1 <C 1010 10^m 11

G(n, a, m) -> E(n', a', m')  (if b(m) > n) (n' < n)
  01 0000^k <C 101a R(m) -> <C 10 10^2k 11 1a R(m + 2^k - 1)

Start -> D(0, 0, 11)  @ Step 85
```

The tricky bit here appears to be preserving a "Reset Invariant" in this transition $$D(n-1, 1, 2^k - 1) \to G(2n, 0, 2^{k-1})$$ where $$n$$ doubles and $$m$$ is cut in half. For example, the trajectory of the TM actually reaches $$G(2, 0, 1)$$ where $$2 > b(1)$$ early on. In fact, this TM repeatedly hits $$G$$ configs which violate the strict invariant $$b(m) > n$$. However, this invariant is overly strict. In fact we only need $$b(m) > 2^k$$ where $$2^k$$ is the largest power of 2 which divides $$n$$. It appears that that is always true. In fact, experimentally, after an initial bit it looks like $$k = 1$$ every time we reach the $$G$$ config ...


### Skelet \#15 (`sk15`)

`sk15` is the exact same machine as `sk26` except starting on a different start state (`sk33`s state `D` or `sk26`s state `E`).

Non-normalized: `1RB1LC_1RE0RB_1LD0LA_1LE---_1LA1RE` (Start state `D`)
TNF Normalized: `1RB---_1RC1LB_1LD1RE_1LB0LD_1RA0RC` (`A` -> `C` -> `E` -> `B` -> `D` -> `A`)

Analysis is the same as for `sk26` except with initial condition: `Start -> D(0, 0, 1) @ Step 21`.


{% include skelet-toc.html %}
