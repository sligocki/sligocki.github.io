---
layout: post
title: Mother of Giants
tags: busy-beaver collatz
---

I would like to introduce you to the Mother of Giants:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LE |
|  B  | 0LC | 0LB |
|  C  | 0LD | 1LC |
|  D  | 1RD | 1RA |
|  E  | **???** | 0LA |

She has an incomplete TM transition table with the undefined transition `E0 -> ???`. For each concrete choice of that transition we'll have one of her children. Like any family, each member is unique, but they also share a lot in common. But why is she Mother of **Giants**? Well, for starters, among her children are the current top 3 BBB(5, 2) champions:

* `E0 -> 0RC` and `E0 -> 0LD` are the machines I [announced on April 2nd](https://groups.google.com/g/busy-beaver-discuss/c/zUiw3G7hR8E/m/kuVi5jdYCgAJ) which quasihalt after >10<sup>14_006</sup> steps
* `E0 -> 0RD` is the machine Nick Drozd [announced on March 16th](https://groups.google.com/g/busy-beaver-discuss/c/KofE0K7_AbQ/m/oPmPNN1UDQAJ) which quasihalts after 10<sup>4_079</sup> steps (Note that in Nick's post, he used a different permutation of states. In his permutation, the transition is `C0 -> 0RE`. However, these machines are isomorphic up to state permutation.)

and this is only the beginning. Some of her children appear to run sooo long it may be difficult for us to prove that they will ever halt. But I hope to convince you that they "probviously" will and so will dwarf all existing champions!

## Analysis of the Mother

Let: <code>F(a, b, c) = 0<sup>∞</sup> <D 0<sup>a</sup> 10<sup>b</sup> 1<sup>c</sup> 0<sup>∞</sup></code>

The Mother of Giants (and all of her children) satisfies the following rules:

* `F(a, b + 2, c)` → `F(a + 7, b, c)`
* `F(a, 1, c + 1)` → `F(a + 6, 0, c)`

* `F(2k + 1, 0, c + 2)` → `F(1, k + 2, c + 1)`

* `F(a, 0, 1)` → `F(1, 0, a + 3)`
* `F(a, 0, 0)` → Spin Out / Quasihalt

* Blank tape → `F(1, 0, 1)` (at step 4)

(I leave proof of these rules as an exercise for the reader. They follow a similar style to those proofs in previous Collatz analysis posts.)

There's one rule missing from this list. What happens after config `F(2k, 0, c + 2)`? This is the one part of the analysis where each child brings their own unique twist!

## Her Children

Among her 20 children (2 symbols to write × 2 directions to move × 5 states to switch to), 8 stand out:

| `E0`→       | `F(2k,0,c+2)`→  | Max reset config (known) | Final config (if known) |
| :---------- | :-------------- | :--------------------- |
| `1RC`/`1LC` | `F(1,k+3,c+1)`  | <code>F(1,0,>3×10<sup>286_575</sup>)</code> | |
| `1LA`       | `F(4,k+1,c+1)`  | <code>F(1,0,>2×10<sup>25_886</sup>)</code> | |
| `0LC`       | `F(2,k+1,c+1)`  | <code>F(1,0,>6×10<sup>12_645</sup>)</code> | |
| `0RC`/`0LD` | `F(1,k+1,c+1)`  | `F(1,0,43_348)`        | <code>F(>2×10<sup>7003</sup>,0,0)</code> |
| `1RD`       | `F(7,k-1,c+1)`* | `F(1,0,39_993)`        | <code>F(>6×10<sup>6489</sup>,0,0)</code> |
| `0RD`       | `F(6,k-1,c+1)`* | `F(1,0,12_601)`        | <code>F(>1×10<sup>2040</sup>,0,0)</code> |

\* Note: Rules with `k-1` only apply when `k ≥ 1`. But this does not have any effect on the analysis in practice because we never see config `F(2, 0, c)` when starting from a blank tape.

## Collatz Behavior of `1RC`

In order to make sense of the third column above, we should spend a little time understanding the behavior of these rules.

Let <code>G(n, m) = F(n, 0, m) = 0<sup>∞</sup> <D 0<sup>n</sup> 1<sup>m</sup> 0<sup>∞</sup></code>

First, we can combine the first two rules in order to eliminate `b` immediately:

* `F(a, 2k, c)` → `F(7k+a, 0, c)` = `G(7k+a, c)`
* `F(a, 2k+1, c+1)` → `F(7k+a, 1, c+1)` → `F(7k+a+6, 0, c)` = `G(7k+a+6,c)`

Going further, it helps to have a specific machine in mind: let's explore the `E0→1RC` machine.

Let <code>G(n, m) = F(n, 0, m) = 0<sup>∞</sup> <D 0<sup>n</sup> 1<sup>m</sup> 0<sup>∞</sup></code>

then:

* `G(4k, m+2)` → `F(1, 2k+3, m+1)` → `G(7k+14, m)`
* `G(4k+1, m+2)` → `F(1, 2k+2, m+1)` → `G(7k+8, m+1)`
* `G(4k+2, m+2)` → `F(1, 2k+4, m+1)` → `G(7k+15, m+1)`
* `G(4k+3, m+2)` → `F(1, 2k+3, m+1)` → `G(7k+14, m)`

* `G(n, 1)` → `G(1, n + 3)`
* `G(n, 0)` → Spin Out / Quasihalt

## Two-stage Collatz Behavior

These `G` rules exhibit Collatz-like behavior, but it is more complicated than the simple Collatz-like behavior described in my [analysis of the BBB(4, 2) champion]({% post_url 2021-07-17-bb-collatz %}) which effectively could have been written as a function of only one argument. But it is also simpler (or at least more regular) than the behavior described in my [analysis of the BBB(5, 2) machine which ran for 10<sup>502</sup> steps]({% post_url 2022-02-22-collatz-complex %}). I call this behavior two-stage.

In stage 1: We apply the first 4 rules repeatedly effectively iterating the Collatz-like function:

* `g(4k) = 7k+14`
* `g(4k+1) = 7k+8`
* `g(4k+2) = 7k+15`
* `g(4k+3) = 7k+14`

until `m < 2`.

Once `m < 2`, we enter stage 2, which is quite simple. If `m = 1` then we reset and start again through stage 1: `G(n, 1)` → `G(1, n + 3)`. However, if `m = 0` then we are finished and the TM quasihalts.

Once a TM has "reset", we know that it will run for a "long" time (stage 1) before it's next opportunity to quasihalt. Specifically, if we reset to config `G(1, m)` then we will iterate (at the very minimum) `(m-1)/2` times before `m < 2` and we enter stage 2 (and thus get an opportunity to quasihalt).

Note: The precise Collatz-like function constants depend on which child we consider, however, the general behavior is the same for all (iterating through stage 1 until `m < 3`, then in stage 2, either resetting or quasihalting).

## Revisiting the Children

| `E0`→       | `F(2k,0,c+2)`→  | Max reset config (known) | Final config (if known) |
| :---------- | :-------------- | :--------------------- |
| `1RC`/`1LC` | `F(1,k+3,c+1)`  | <code>G(1, >3×10<sup>286_575</sup>)</code> | |
| `1LA`       | `F(4,k+1,c+1)`  | <code>G(1, >2×10<sup>25_886</sup>)</code> | |
| `0LC`       | `F(2,k+1,c+1)`  | <code>G(1, >6×10<sup>12_645</sup>)</code> | |
| `0RC`/`0LD` | `F(1,k+1,c+1)`  | `G(1, 43_348)`        | <code>G(>2×10<sup>7003</sup>, 0)</code> |
| `1RD`       | `F(7,k-1,c+1)`* | `G(1, 39_993)`        | <code>G(>6×10<sup>6489</sup>, 0)</code> |
| `0RD`       | `F(6,k-1,c+1)`  | `G(1, 12_601)`        | <code>G(>1×10<sup>2040</sup>, 0)</code> |

So this explains column 3 in the table of children above. It lists the largest "reset" config that I have found by direct computation. For the bottom three rows, the value of `m` after the final reset was small enough (less than 100k) that I was able to directly compute through the final stage 1 iteration and discover that these machines quasihalted. However, for the top three rows, the values of `m` are far too large to compute through the next stage 1 iteration directly (It would take > 10<sup>10_000</sup> iterations!)

## Are the Giants Ghosts?

So, these top 3 rows (describing 4 Turing Machines) which certainly run longer than the reigning BBB(5, 2) champions (their own siblings). But do they ever quasihalt? Are they Giants or merely Ghosts? As far as I know, there are no Mathematical techniques yet to prove that they will never quasihalt and that the only techniques to prove that they do quasihalt is direct computation which is not practical without an exponential speed-up for the top 3 rows.

However, it **feels** that they certainly must all eventually quasihalt. If they did not, that we would mean that we enter stage 2 an infinite number of times, each time with `m = 1` (never `m = 0`)! If you examine a number of example orbits, you will see that each time they enter stage 2 about half reset and half quasihalt! Of course, this sort of statement is a bit wishy-washy for Mathematics, but at the same time, if I were in a position to wager on whether or not these quasihalted, I would unquestionably wager that they do. All available evidence we have suggests it. I would be shocked to discover that they somehow manage to dance perfectly into the reset lane every time! In the [words of John Conway](https://doi.org/10.4169/amer.math.monthly.120.03.192): These TMs "probviously" quasihalt.

But could we call them champions based on such a hand-wavy argument? It feels a bit premature. And yet, it also feels improper to say the previous record holders are still champions when I feel so sure that their 4 siblings will all beat them. But does that mean that there are no champions? I think we have entered an interesting new phase of the Busy Beaver competition in which we have machines we believe run far longer than all proven machines ... but we cannot prove it.

That said, I hope to hear one of you delightful readers tell me that, in fact, we can accelerate the simulation of these Collatz-like functions enough to find when then quasihalt. Let me know if you have any ideas!
