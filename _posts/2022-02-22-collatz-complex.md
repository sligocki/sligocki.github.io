---
layout: post-custom
title: BBB Complex Collatz-like Behavior
tags: busy-beaver collatz
---

Almost all existing Busy Beaver champions (and runner ups) exhibit Collatz-like behavior. Pascal Michel has a [large collection](https://bbchallenge.org/~pascal.michel/beh.html) of these analyses for existing and previous BB champions. I previously [analyzed the BBB(4, 2) champion]({% post_url 2021-07-17-bb-collatz %}) (it is still the reigning champion 7 months later) which fit into the Collatz-like model in a pleasingly simple way. Today, I'll be analyzing my newly discovered [BBB(5, 2) champion]({% post_url 2021-07-17-bb-collatz %}) which runs for over 10<sup>502</sup> steps before quasihalting. This machine exhibits more complex behavior, but still, it is not ["Spaghetti Code"](https://nickdrozd.github.io/2021/01/26/spaghetti-code-conjecture.html).

## The Machine

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LC |
|  B  | 1RC | 0RD |
|  C  | 0LB | 0RC |
|  D  | 0RE | 1RD |
|  E  | 1LE | 1LA |

`1RB 1LC  1RC 0RD  0LB 0RC  0RE 1RD  1LE 1LA`

As discussed in my announcement, it runs > 10<sup>502</sup> steps before quasihalting. At that number of steps (> a googol<sup>5</sup>) it is impossible to simulate directly. The only option we have for analyzing this machine is by finding patterns in it's behavior that allow us to exponentially accelerate our simulation of it.

## Rule Steps

I will jump straight into defining the "Rule Steps" of this machine. For a more step-by-step discussion of how we get to rule steps, including the description of tape compression notation and chain steps, please read my original article: ["Collatz-like behavior of Busy Beavers"]({% post_url 2021-07-17-bb-collatz %}).

### Common Configuration

This analysis will be built around a common configuration:

<code>... 1<sup>n</sup> <E 1<sup>m</sup> 0<sup>∞</sup></code>

which appears again and again as this TM runs. Note that this is only a partial tape specification, there could be many different patterns in the `...` section (we'll dig more into that later in the "Remainder Rules" section). As a shorthand, we will denote this partial configuration as:

<code>f(n, m) = 1<sup>n</sup> <E 1<sup>m</sup> 0<sup>∞</sup></code>

So, for example, <code>0<sup>∞</sup> 11101110 f(n, m)</code> would denote the full configuration <code>0<sup>∞</sup> 11101110 1<sup>n</sup> <E 1<sup>m</sup> 0<sup>∞</sup></code>.

### The Basic Rule

`f(n+3, m)` -> `f(n, m+5)` in `2m + 15` steps.

Proof:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 1<sup>n+3</sup> | <b><E</b> | 1<sup>m</sup> 0<sup>∞</sup> |
|          1 | 1<sup>n+2</sup> | <b><A</b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|          2 | 1<sup>n+1</sup> | <b><C</b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          3 | 1<sup>n</sup> 0 | <b>C></b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|     m +  5 | 1<sup>n</sup> 0<sup>m+3</sup> | <b>C></b> | 0<sup>∞</sup> |
|     m +  6 | 1<sup>n</sup> 0<sup>m+3</sup> | <b><B</b> | 0<sup>∞</sup> |
|     m +  7 | 1<sup>n</sup> 0<sup>m+2</sup> 1 | <b>C></b> | 0<sup>∞</sup> |
|     m +  8 | 1<sup>n</sup> 0<sup>m+2</sup> 1 | <b><B</b> | 0<sup>∞</sup> |
|     m +  9 | 1<sup>n</sup> 0<sup>m+3</sup> | <b>D></b> | 0<sup>∞</sup> |
|     m + 10 | 1<sup>n</sup> 0<sup>m+4</sup> | <b>E></b> | 0<sup>∞</sup> |
|     m + 11 | 1<sup>n</sup> 0<sup>m+4</sup> | <b><E</b> | 1 0<sup>∞</sup> |
|    2m + 15 | 1<sup>n</sup> | <b><E</b> | 1<sup>m+5</sup> 0<sup>∞</sup> |

If you were to jump to any random point in the execution of this TM, you would almost certainly see it in the middle of evaluating this specific rule. It goes through eating 3 `1`s from the left and adding 5 `1`s to the right and repeats until the number of `1`s on the left gets to be less than 3.

### Repeating Basic Rule

As with the BBB(4, 2) analysis, we can repeatedly apply this rule an arbitrary number of times in one simulator step. Specifically, we have a new rule:

`f(3k + r, m)` -> `f(r, m + 5k)`

### Remainder Rules

OK, but what happens when `n < 3`? Well now we have to consider the details of what's in the `...` section on the left side of the tape.

#### Case `n = 1`

`0 f(1, m)` -> `10 f(m, 2)` in `m + 6` steps.

Proof: 

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 0 1   | <b><E</b> | 1<sup>m</sup> 0<sup>∞</sup> |
|          1 | 0     | <b><A</b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|          2 | 1     | <b>B></b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|          3 | 1 0   | <b>D></b> | 1<sup>m</sup> 0<sup>∞</sup> |
|     m +  3 | 1 0 1<sup>m</sup>   | <b>D></b> | 0<sup>∞</sup> |
|     m +  4 | 1 0 1<sup>m</sup> 0 | <b>E></b> | 0<sup>∞</sup> |
|     m +  5 | 1 0 1<sup>m</sup> 0 | <b><E</b> | 1 0<sup>∞</sup> |
|     m +  6 | 1 0 1<sup>m</sup>   | <b><E</b> | 1<sup>2</sup> 0<sup>∞</sup> |

#### Case `n = 2`

`00 f(2, m)` -> `010 f(m+1, 2)` in `m + 14` steps.

Proof:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 0 0 1 1 | <b><E</b> | 1<sup>m</sup> 0<sup>∞</sup> |
|          1 | 0 0 1   | <b><A</b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|          2 | 0 0     | <b><C</b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          3 | 0       | <b><B</b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          4 | 1       | <b>C></b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          5 | 1       | <b><B</b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          6 | 0       | <b>D></b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          7 | 0 0     | <b>E></b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          8 | 0 0     | <b><A</b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          9 | 0 1     | <b>B></b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|         10 | 0 1 0   | <b>D></b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|     m + 11 | 0 1 0 1<sup>m+1</sup>   | <b>D></b> | 0<sup>∞</sup> |
|     m + 12 | 0 1 0 1<sup>m+1</sup> 0 | <b>E></b> | 0<sup>∞</sup> |
|     m + 13 | 0 1 0 1<sup>m+1</sup> 0 | <b><E</b> | 1 0<sup>∞</sup> |
|     m + 14 | 0 1 0 1<sup>m+1</sup>   | <b><E</b> | 1<sup>2</sup> 0<sup>∞</sup> |

`10 f(2, m)` -> `010 f(m+1, 2)` in `m + 12` steps.

Proof:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 1 0 1 1 | <b><E</b> | 1<sup>m</sup> 0<sup>∞</sup> |
|          1 | 1 0 1   | <b><A</b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|          2 | 1 0     | <b><C</b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          3 | 1       | <b><B</b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          4 | 0       | <b>D></b> | 0 1<sup>m+2</sup> 0<sup>∞</sup> |
|          5 | 0 0     | <b>E></b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          6 | 0 0     | <b><A</b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          7 | 0 1     | <b>B></b> | 1<sup>m+2</sup> 0<sup>∞</sup> |
|          8 | 0 1 0   | <b>D></b> | 1<sup>m+1</sup> 0<sup>∞</sup> |
|     m +  9 | 0 1 0 1<sup>m+1</sup>   | <b>D></b> | 0<sup>∞</sup> |
|     m + 10 | 0 1 0 1<sup>m+1</sup> 0 | <b>E></b> | 0<sup>∞</sup> |
|     m + 11 | 0 1 0 1<sup>m+1</sup> 0 | <b><E</b> | 1 0<sup>∞</sup> |
|     m + 12 | 0 1 0 1<sup>m+1</sup>   | <b><E</b> | 1<sup>2</sup> 0<sup>∞</sup> |

#### Case `n = 0`

`0 f(0, m)` -> `f(0, m+1)` in 1 step

Proof:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | `0` | <b><E</b> | 1<sup>m</sup> 0<sup>∞</sup> |
|          1 |   | <b><E</b> | 1<sup>m+1</sup> 0<sup>∞</sup> |

As a result, notice that this means that if we are in configuration:

<code>0<sup>∞</sup> f(0, m)</code> = <code>0<sup>∞</sup> <b><E</b> 1<sup>m</sup> 0<sup>∞</sup></code>

That means that the machine has reached Chain Recurrence and it will remain in state `E` forever after, moving left, writing `1`s (because of transition `E0 -> 1LE`).

#### Final Case

Finally, note that

`1 f(n, m)` = <code>1 1<sup>n</sup> <b><E</b> 1<sup>m</sup> 0<sup>∞</sup></code> = `f(n+1, m)`

## Collatz-like Function

Collecting all these rules together we get:

* `f(3k + r, m)` -> `f(r, m + 5k)`
* `1 f(n, m)` = `f(n+1, m)`
* `0 f(1, m)` -> `10 f(m, 2)`
* `00 f(2, m)` -> `010 f(m+1, 2)`
* `10 f(2, m)` -> `010 f(m+1, 2)`
* `0 f(0, m)` -> `f(0, m+1)`

* <code>0<sup>∞</sup> h(0, m)</code> -> Quasihalt

We can simplify this a bit by combining the first rule into the rest to get:

* `1 f(n, m)` = `f(n+1, m)`
* `0 f(3k + 1, m)` -> `10 f(5k + m, 2)`
* `00 f(3k + 2, m)` -> `010 f(5k + m + 1, 2)`
* `10 f(3k + 2, m)` -> `010 f(5k + m + 1, 2)`
* `0 f(3k, m)` -> `f(0, 5k + m + 1)`

It turns out we can even codify this further in order to completely convert it into a Collatz-like function. Specifically, consider the general configuration:

`g(x, n, m)` = <code> 0<sup>∞</sup> bin(x) 1<sup>n</sup> <E 1<sup>m</sup> 0<sup>∞</sup></code> = <code> 0<sup>∞</sup> bin(x) f(n, m)</code>

where `bin(x)` means writing `x` in binary in the standard big-endian notation (most significant bit on left, least significant bit on right). Ex: `bin(18)` = `10010`.

It might not be at first clear why we are using binary encoding here. But consider what happens when we consider an example:

`g(2x, 3k + 1, m)` = <code> 0<sup>∞</sup> bin(2x) f(3k + 1, m)</code> = <code> 0<sup>∞</sup> bin(x) 0 f(3k + 1, m)</code> -> <code> 0<sup>∞</sup> bin(x) 10 f(5k + m, 2)</code> = <code> 0<sup>∞</sup> bin(4x+2) f(5k + m, 2)</code> = `g(4x+2, 5k + m, 2)`

So, in fact, each of the rules above can be converted completely into this `g` context:

* `g(2x+1, n, m)` = `g(x, n+1, m)`
* `g(2x, 3k + 1, m)` -> `g(4x+2, 5k + m, 2)`
* `g(4x, 3k + 2, m)` -> `g(8x + 2, 5k + m + 1, 2)`
* `g(4x + 2, 3k + 2, m)` -> `g(8x + 2, 5k + m + 1, 2)`
* `g(0, 3k, m)` -> Quasihalt
* `g(2x, 3k, m)` -> `g(x, 0, 5k + m + 1)`

## Collatz Orbit from Blank Tape

Starting our TM from a blank tape at step 8 it is in configuration:

<code>0<sup>∞</sup> 1<sup>1</sup> <b><E</b> 1<sup>3</sup> 0<sup>∞</sup></code> = `g(0, 1, 3)`

Starting from this point the orbit of the Collatz function is:

| # Iterations | Configuration |
| -----------: | :------------ |
|            0 | g(0, 1, 3) |
|            1 | g(2, 3, 2) |
|            2 | g(1, 0, 8) |
|            3 | g(0, 1, 8) |
|            4 | g(2, 8, 2) |
| ... | ... |
|         2257 | g(14, 3, ~2.4 × 10<sup>251</sup>) |
|         2258 | g(7, 0, ~2.4 × 10<sup>251</sup>) |
|         2259 | g(3, 1, ~2.4 × 10<sup>251</sup>) |
|         2260 | g(1, 2, ~2.4 × 10<sup>251</sup>) |
|         2261 | g(0, 3, ~2.4 × 10<sup>251</sup>) |
|         2262 | Quasihalt |

In other words, we iterate this function `2262` times before the machine Quasihalts.

## Intuition

In a way, I am done now. I have given you a precise Collatz-like function which exactly simulates this TM ... but what is really going on here? In general, it feels like the value of having a Collatz analysis is that it sheds some light on what the TM is "actually doing". But (at least for me) seeing the definition of this `g` and the 2262 iterations it takes to Quasihalt is not especially illuminating. Instead I actually find the definition of `f` to be more helpful in understanding what this machine "does". Let me restate the `f` rules in yet another way:

* `f(3k + r, m)` -> `f(r, m + 5k)`
* `0 f(1, m)` -> `10 f(m, 2)`
* `?0 f(2, m)` -> `010 f(m+1, 2)` (for `?` either `0` or `1`)
* <code>0 1<sup>a</sup> 0<sup>b</sup> f(0, m)</code> -> `0 f(a, m+b)`
* <code>0<sup>∞</sup> f(0, m)</code> -> Quasihalt

So, we repeatedly iterate a Collatz-like function on `(n, m)` doing something different with the left side of the tape depending on the remainder of `n` mod 3. If the remainder is `1` or `2`, then we expand the size of the left side of the tape by 1 symbol (`0 -> 10` or `?0 -> 010`). If the remainder is `0`, then we reduce the size of the left side of the tape. In order to quasihalt, the TM must reduce the left side of the tape down to empty and then have a remainder of `0`. The question is, will the left side of the tape be reduced to empty and have remainder `0` for `n`? Or will it grow endlessly?

By looking at the details of this specific orbit, it appears that these two forces (growth and reduction) are pretty evenly balanced. The tape grows a lot at first and then stays long for thousands of iterations before finally crashing down to empty.

See here, for example, where I show only the iterations where `n < 3` (just the remainder).

| Iteration # | Left Tape | n | m |
| -: | -: | -: | -: |
| 0 |  | 1 | 3 |
| 1 | `1` | 0 | 7 |
| 2 |  | 1 | 8 |
| 3 | `1` | 2 | 12 |
| 4 | `01` | 1 | 22 |
| 5 | `011` | 1 | 37 |
| 6 | `0111` | 1 | 62 |
| 7 | `01111` | 2 | 102 |
| 8 | `011101` | 1 | 172 |
| 9 | `0111011` | 1 | 287 |
| 10 | `01110111` | 2 | 477 |
| 11 | `011101101` | 1 | 797 |
| 12 | `0111011011` | 2 | 1327 |
| 13 | `01110110101` | 2 | 2212 |
| 14 | `011101101001` | 2 | 3687 |
| 15 | `0111011010001` | 1 | 6147 |
| 16 | `01110110100011` | 0 | 10247 |
| 17 | `01110110100` | 2 | 10248 |
| 18 | `011101101001` | 1 | 17082 |
| 19 | `0111011010011` | 0 | 28472 |
| 20 | `0111011010` | 2 | 28473 |
| 21 | `01110110101` | 1 | 47457 |
| 22 | `011101101011` | 0 | 79097 |
| 23 | `011101101` | 2 | 79098 |
| 24 | `0111011001` | 1 | 131832 |
| 25 | `01110110011` | 0 | 219722 |
| 26 | `01110110` | 2 | 219723 |
| 27 | `011101101` | 1 | 366207 |
| 28 | `0111011011` | 0 | 610347 |
| 29 | `0111011` | 2 | 610348 |
| 30 | `01110101` | 2 | 1017247 |
| 31 | `011101001` | 2 | 1695412 |
| 32 | `0111010001` | 2 | 2825687 |
| 33 | `01110100001` | 0 | 4709482 |
| 34 | `011101000` | 1 | 4709483 |
| 35 | `0111010001` | 2 | 7849137 |
| 36 | `01110100001` | 1 | 13081897 |
| 37 | `011101000011` | 1 | 21803162 |
| 38 | `0111010000111` | 2 | 36338602 |
| 39 | `01110100001101` | 2 | 60564337 |
| 40 | `011101000011001` | 2 | 100940562 |
| ...| ...| ... | ... |
| 1498 | `011101101001010001` | 2 | |
| 1499 | `0111011010010100001` | 1 | |
| 1500 | `01110110100101000011` | 1 | |
| 1501 | `011101101001010000111` | 0 | |
| 1502 | `01110110100101000` | 0 | |
| 1503 | `011101101001` | 1 | |
| 1504 | `0111011010011` | 0 | |
| 1505 | `0111011010` | 2 | |
| 1506 | `01110110101` | 0 | |
| 1507 | `011101101` | 1 | |
| 1508 | `0111011011` | 1 | |
| 1509 | `01110110111` | 1 | |
| 1510 | `011101101111` | 0 | |
| 1511 | `0111011` | 1 | |
| 1512 | `01110111` | 2 | |
| 1513 | `011101101` | 0 | |
| 1514 | `0111011` | 1 | |
| 1515 | `01110111` | 0 | |
| 1516 | `0111` | 0 | |
| 1517 | `` | 0 | |
