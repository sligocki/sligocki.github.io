---
layout: post-custom
title: BB Counters and Proof by Induction
tags: busy-beaver
---

In our codebase, we have some extremely powerful Busy Beaver techniques for accelerating Turing Machine simulation. But they are all dependent upon effective [Tape Compression]({% post_url 2021-07-17-bb-collatz %}#tape-compression). If we cannot compress the tape, we cannot hope to gain much acceleration at all. And the bane of our run-length encoding tape compression are *Counters*.


## Naming

As far as I know the name *Counter* was coined for these machines by Allen Brady in [his 1983 paper](https://doi.org/10.1090/S0025-5718-1983-0689479-6) where he proved BB(4) = 107. These are TMs which simulate "Binary Counters" by repeatedly "adding one" to a binary representation of an integer stored on the tape. These TMs take an exponentially increasing amount of time to expand the tape each additional increase. bbchallenge.org calls them "Exponential Counters".


## Example

Consider ([TM #11,004,366](https://bbchallenge.org/11004366) on bbchallenge.org):

`1RB1LA_0LC0RB_0LD0LB_1RE---_1LE1LA`

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LA |
|  B  | 0LC | 0RB |
|  C  | 0LD | 0LB |
|  D  | 1RE | --- |
|  E  | 1LE | 1LA |

If we look at this machine only when it is in state B, reading a 0 off the right side of the tape, we see:

|   Step   |          Tape          |   Binary Counter Value   |
| -------: | :--------------------: | -----------------------: |
|    **11**| `...000001 00 B> 0...` |                        1 |
|    **21**| `...000010 00 B> 0...` |                        2 |
|      29  | `...000011 00 B> 0...` |                        3 |
|    **41**| `...000100 00 B> 0...` |                        4 |
|      49  | `...000101 00 B> 0...` |                        5 |
|      59  | `...000110 00 B> 0...` |                        6 |
|      67  | `...000111 00 B> 0...` |                        7 |
|    **81**| `...001000 00 B> 0...` |                        8 |
|      89  | `...001001 00 B> 0...` |                        9 |
|      99  | `...001010 00 B> 0...` |                       10 |
|     107  | `...001011 00 B> 0...` |                       11 |
|     119  | `...001100 00 B> 0...` |                       12 |
|     127  | `...001101 00 B> 0...` |                       13 |
|     137  | `...001110 00 B> 0...` |                       14 |
|     145  | `...001111 00 B> 0...` |                       15 |
|   **161**| `...010000 00 B> 0...` |                       16 |

As advertised, this TM increments a binary counter and takes an exponentially increasing amount of time to expand the tape each step (the bolded step numbers).

This tape cannot be compressed at all times because over the course of time it will contain every finite binary sequence including the super-non-compressible ones like: `101001000100001` ... whatcha going to do with that? Certainly not compress it using run-length encoding. And if you can't compress it using run-length encoding, my simulator cannot prove rules and cannot accelerate it's simulation.


### Proving Non-halting

But it is possible to prove Counters are non-halting. It's even possible with a *small* modification of our normal [Proof rules]({% post_url 2021-07-17-bb-collatz %}#rule-step). The modification we need is: [Proof by Induction](https://en.wikipedia.org/wiki/Mathematical_induction)!

Consider the following rule: `P(n)`: <code>0<sup>n</sup> 1 00 B> 0 -> 1<sup>n+1</sup> 00 B> 0</code>.

The base case is obvious. `P(0)`: <code>0<sup>0</sup> 1 00 B> 0 -> 1<sup>1</sup> 00 B> 0</code> in `0` steps (the left and right sides are the same tape).

Now, for the inductive case: Let us assume that `P(k)` is true: <code>0<sup>k</sup> 1 00 B> 0 -> 1<sup>k+1</sup> 00 B> 0</code> in `f(k)` steps. Then:

|            Step # |         Left tape |   State   | Right tape        | Via |
| ----------------: | ----------------: | :-------: | :---------------- | :-: |
|                 0 | <code>0 0<sup>k</sup> 1 00</code> | <b>B></b> | <code>0</code> |            |
|   f(k) +        0 | <code>0 1<sup>k+1</sup> 00</code> | <b>B></b> | <code>0</code> | `P(k)`     |
|   f(k) +        1 | <code>0 1<sup>k+1</sup> 00</code> | <b><C</b> | <code>0</code> | Base Step  |
|   f(k) +        2 | <code>0 1<sup>k+1</sup> 0</code> | <b><D</b> | <code>00</code> | Base Step  |
|   f(k) +        3 | <code>0 1<sup>k+1</sup> 1</code> | <b>E></b> | <code>00</code> | Base Step  |
|   f(k) +        4 | <code>0 1<sup>k+1</sup> 1</code> | <b><E</b> | <code>10</code> | Base Step  |
|   f(k) +        5 | <code>0 1<sup>k+1</sup></code> | <b><A</b> | <code>110</code>  | Base Step  |
|   f(k) +   k +  6 | <code>0</code> | <b><A</b> | <code>1<sup>k+1</sup> 110</code>  | Chain Step |
|   f(k) +   k +  7 | <code>1</code> | <b>B></b> | <code>1<sup>k+1</sup> 110</code>  | Base Step  |
|   f(k) + 2 k + 10 | <code>1 0<sup>k+1</sup> 00</code> | <b>B></b> | <code>0</code> | Chain Step |
|   f(k) + 2 k + 11 | <code>1 0<sup>k+1</sup> 00</code> | <b><C</b> | <code>0</code> | Base Step  |
|   f(k) + 2 k + 12 | <code>1 0<sup>k+1</sup> 0</code> | <b><D</b> | <code>00</code> | Base Step  |
|   f(k) + 2 k + 13 | <code>1 0<sup>k+1</sup> 1</code> | <b>E></b> | <code>00</code> | Base Step  |
|   f(k) + 2 k + 14 | <code>1 0<sup>k+1</sup> 1</code> | <b><E</b> | <code>10</code> | Base Step  |
|   f(k) + 2 k + 15 | <code>1 0<sup>k+1</sup></code> | <b><A</b> | <code>110</code>  | Base Step  |
|   f(k) + 2 k + 16 | <code>1 0<sup>k</sup> 1</code> | <b>B></b> | <code>110</code>  | Base Step  |
|   f(k) + 2 k + 18 | <code>1 0<sup>k</sup> 1 00</code> | <b>B></b> | <code>0</code> | Chain Step |
| 2 f(k) + 2 k + 18 | <code>1 1<sup>k+1</sup> 00</code> | <b>B></b> | <code>0</code> | `P(k)`     |

Thus we have proven `P(k+1)`: <code>0<sup>k+1</sup> 1 00 B> 0 -> 1<sup>k+2</sup> 00 B> 0</code> in `f(k+1) = 2 f(k) + 2 k + 18` steps.

Thus for any `n`, `P(n)` is true: <code>0<sup>n</sup> 1 00 B> 0 -> 1<sup>n+1</sup> 00 B> 0</code> and thus this TM will obviously never halt since it will write an unbounded number of `1`s.


### Counting Steps

To get the exact value of the steps function `f(n)`, we need to solve the recursive relation:

 * `f(0) = 0`
 * `f(n+1) = 2 f(n) + 2 n + 18`

First, let `g(n) = f(n) + 2n`, then:

 * `g(0) = f(0) + 0 = 0`
 * `g(n+1) = f(n+1) + 2(n+1) = 2 f(n) + 2n + 18 + 2n + 2 = 2 (g(n) - 2n) + 4n + 20 = 2 g(n) + 20`

Second let `h(n) = g(n) + 20`, then:

 * `h(0) = 20`
 * `h(n+1) = g(n+1) + 20 = 2 g(n) + 20 + 20 = 2 (h(n) - 20) + 40 = 2 h(n)`

Now, `h(n)` is easy to solve: <code>h(n) = 20 2<sup>n</sup></code> so

 * <code>f(n) = g(n) - 2n = h(n) - 2n - 20 = 20 2<sup>n</sup> - 2n - 20</code>.

which computes the following step estimates:

| Step Equation | Steps |          Tape          |
| :-----------: | ----: | :--------------------: |
|               |    11 | <code>0<sup>∞</sup> 1<sup>1</sup> 00 B> 0<sup>∞</sup></code> |
|  `11 + f(1)`  |    29 | <code>0<sup>∞</sup> 1<sup>2</sup> 00 B> 0<sup>∞</sup></code> |
|  `11 + f(2)`  |    67 | <code>0<sup>∞</sup> 1<sup>3</sup> 00 B> 0<sup>∞</sup></code> |
|  `11 + f(3)`  |   145 | <code>0<sup>∞</sup> 1<sup>4</sup> 00 B> 0<sup>∞</sup></code> |

which does match the results above.


## Inductive Rules vs. Meta Rules

In our current simulator, we are able to prove:

 * *Basic Rules*: Rules whose proofs consist of all base (or chain) steps or
 * *Meta Rules* (aka *Recursive Rules*): Rules whose proofs include also previously proven rule steps.

This proof requires a more powerful type: *Inductive Rules* - rules whose proofs include rule steps for (smaller versions) of this Rule being currently proven.


## Automated proofs

Here I showed a hand-written proof for accelerating a counter using Induction. I believe that this style of inductive proof generalizes to work for most variations of Counters. But, can we automate the detection and proving of these sorts of rules (like we do for Christmas Tree rules)? I think it should be possible, but I'm not sure about the details. **This is future work**: How do you detect a counter, guess the rule and know now long to simulate in order to prove the rule inside your system?

In order to prove `BB(4)`, Brady did automate Counter proving 40 years ago. Like his Christmas tree detection, he built a sort of "cellular decomposition" of the tape into `<0>` and `<1>` blocks and then proved by induction that for any TM which could be decomposed this way, it will never halt.

The downside of these sorts of strict decompositions is that there turn out to be many Counter-like TMs that didn't fit into a single decomposition and so several decompositions needed to be hand-made to deal with different variations. A similar issue came up for Christmas Tree cellular decomposition leading to the need to define new decompositions for things like *Leaning Trees* and *Trees with Shadow*, etc.

Tape compression, chain steps and our current automated proof systems have generally been powerful enough to prove all variations of Christmas Trees without the need for variations-specific code. In my opinion, it has made proving Christmas Trees much simpler and less error prone by producing a single general system which happens to work with all of these variations.

I would like to see something similar happen with automating counter proving and my hope is that it is possible by following the pattern I layed out above: Running an *Inductive Proof* that allows applying smaller versions of the rule being proven itself. My hope is that this would be general enough to cover all variations of counters without the need to each variation to have it's own decomposition and proof.


## Attribution

I think that my idea to prove Counters via induction is not original. In fact, reading [Allen Brady's in 1983 paper](https://doi.org/10.1090/S0025-5718-1983-0689479-6) today, I noticed that he mentions that he has proven counters via induction. However, I am not aware of any previous description of *Inductive Rules* as akin to *Meta Rules* like I describe above.
