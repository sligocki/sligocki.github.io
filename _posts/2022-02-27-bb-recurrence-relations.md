---
layout: post
title: Busy Beavers and Recurrence Relations
tags: busy-beaver collatz
---

While analyzing the behavior of the latest `BBB(3, 3)` champion, I found that I had to brush up my skills on computing exact formulas for [recurrence relations](https://en.wikipedia.org/wiki/Recurrence_relation). Let me explain:

## The Machine

|     |  0  |  1  |  2  |
| :-: | :-: | :-: | :-: |
|  A  | 1RB | 0LB | 1LA |
|  B  | 2LC | 2LB | 2LB |
|  C  | 2RC | 2RA | 0LC |

It quasihalts at step 724193411946051617510910916281388064798340875589283913992444770 ($$> 7.2 \times 10^{62}$$) with respect to state `B`. I [announced this machine](https://groups.google.com/g/busy-beaver-discuss/c/ET4Q_ywWPUE) on last Tuesday on a new Busy Beaver discussion mailing list.

It turns out that this machine has Collatz-like behavior, but requires an extra level of reasoning to analyze.

## Level 1

If you simulate this TM using [tape compression]({% post_url 2021-07-17-bb-collatz %}#tape-compression) and [chain steps]({% post_url 2021-07-17-bb-collatz %}#chain-step) you will probably start noticing the following pattern repeat over and over again:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 0<sup>inf</sup> | <b><C</b> | 0<sup>a</sup> 2<sup>b+1</sup> ... |
|          1 | 0<sup>inf</sup> 2 | <b>C></b> | 0<sup>a</sup> 2<sup>b+1</sup> ... |
|      a + 1 | 0<sup>inf</sup> 2<sup>a+1</sup> | <b>C></b> | 2<sup>b+1</sup> ... |
|      a + 2 | 0<sup>inf</sup> 2<sup>a+1</sup> | <b><C</b> | 0 2<sup>b</sup> ... |
|     2a + 3 | 0<sup>inf</sup> | <b><C</b> | 0<sup>a+2</sup> 2<sup>b</sup> ... |

This is our first rule:

* Rule 1: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 2<sup>b+1</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a+2</sup> 2<sup>b</sup> ...</code> in `2a + 3` steps.

### Repeated Application

Now, we can keep repeating this rule over and over again as long as we still have any `2`s left (`b > 0`). I will write this as:

<code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>0</sub></sup> 2<sup>b<sub>0</sub></sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>1</sub></sup> 2<sup>b<sub>1</sub></sup> ...</code> -> ...  -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>k</sub></sup> 2<sup>b<sub>k</sub></sup> ...</code>

Where

* <code>a<sub>0</sub> = a</code>
* <code>a<sub>k+1</sub> = a<sub>k</sub> + 2</code>
* <code>b<sub>0</sub> = b</code>
* <code>b<sub>k+1</sub> = b<sub>k</sub> - 1</code>

That is, <code>a<sub>k</sub></code> and <code>b<sub>k</sub></code> are the values of `a` and `b` respectively after applying Rule 1 `k` times. These are our first recurrence relations:

A recurrence relation is a sequence of numbers where the next value in the sequence is determined by applying some function to the previous value of the sequence (or sometimes multiple previous values, like in the Fibonacci sequence ... but in our case we will only ever look at a single previous value). In our case, the definition of <code>a<sub>k+1</sub></code> is defined using <code>a<sub>k</sub></code>. Our goal is to "solve" the recurrence relations, which means to develop an equation for <code>a<sub>k</sub></code> which is not recursive (doesn't depend upon previous values of <code>a<sub>k</sub></code>).

This first recurrence relation is pretty easy to solve. Each iteration of Rule 1 just adds 2 to `a` and subtracts 1 from `b`, so after `k` iterations, we get <code>a<sub>k</sub> = a + 2 k</code> and <code>b<sub>k</sub> = b - k</code>. We can keep applying Rule 1 as long as `b > 0`, in other words, until `k = b`, leaving us in configuration:

<code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>b</sub></sup> 2<sup>b<sub>b</sub></sup> ...</code> = <code>0<sup>inf</sup> <b><C</b> 0<sup>a+2b</sup> 2<sup>0</sup> ...</code>

Therefore, we can write out completely the repeated version of Rule 1 as:

Rule 1x: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 2<sup>b</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a+2b</sup> 2<sup>0</sup> ...</code>

How many steps will it take? Well, it takes <code>2a<sub>k</sub> + 3</code> steps at each iteration, so in total it will take:

$$ \sum_{k=0}^{b-1}(2 a_k + 3) = \sum_{k=0}^{b-1}(2 (a + 2 k) + 3) = 4 \sum_{k=0}^{b-1} k + (2a + 3) \sum_{k=0}^{b-1} 1 $$

$$ = 2 b (b-1) + (2a + 3) b = 2 b^2 + (2a + 1) b $$

So fully stated:

* Rule 1x: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 2<sup>b</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a+2b</sup> 2<sup>0</sup> ...</code> in <code>2 b<sup>2</sup> + (2a + 1) b</code> steps

## Level 2

So now that we can quickly clear all the `2`s out, what happens next? Well here's the next situation I see repeatedly happening:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 0<sup>inf</sup> | <b><C</b> | 0<sup>a</sup> 1<sup>c+2</sup> ... |
|          1 | 0<sup>inf</sup> 2 | <b>C></b> | 0<sup>a</sup> 1<sup>c+2</sup> ... |
|      a + 1 | 0<sup>inf</sup> 2<sup>a+1</sup> | <b>C></b> | 1<sup>c+2</sup> ... |
|      a + 2 | 0<sup>inf</sup> 2<sup>a+2</sup> | <b>A></b> | 1<sup>c+1</sup> ... |
|      a + 3 | 0<sup>inf</sup> 2<sup>a+2</sup> | <b><B</b> | 0 1<sup>c</sup> ... |
|     2a + 5 | 0<sup>inf</sup> | <b><B</b> | 2<sup>a+2</sup> 0 1<sup>c</sup> ... |
|     2a + 6 | 0<sup>inf</sup> | <b><C</b> | 2<sup>a+3</sup> 0 1<sup>c</sup> ... |
|          * | 0<sup>inf</sup> | <b><C</b> | 0<sup>2a+6</sup> 0 1<sup>c</sup> ... |

Note: The last step is accomplished by applying Rule 1x to go from: <code>0<sup>inf</sup> <b><C</b> 0<sup>0</sup> 2<sup>a+3</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>2a+6</sup> 2<sup>0</sup> ...</code> in <code>2 (a+3)<sup>2</sup> + (2 * 0 + 1) (a+3)</code> = <code>2 a<sup>2</sup> + 13 a + 21</code> steps

So we get:

* Rule 2: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>c+2</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>2a+7</sup> 1<sup>c</sup> ...</code> in <code>2 a<sup>2</sup> + 15 a + 27</code> steps

### Non-trivial Recurrence Relation

Just like with Rule 1, we can repeatedly apply Rule 2:

<code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>0</sub></sup> 1<sup>c<sub>0</sub></sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>1</sub></sup> 1<sup>c<sub>1</sub></sup> ...</code> -> ...  -> <code>0<sup>inf</sup> <b><C</b> 0<sup>a<sub>k</sub></sup> 1<sup>c<sub>k</sub></sup> ...</code>

Where Rule 2 tells us that:

* <code>a<sub>k+1</sub> = 2 a<sub>k</sub> + 7</code>
* <code>c<sub>k+1</sub> = c<sub>k</sub> - 2</code>

<code>c<sub>k</sub></code> is just as easy as before to calculate: <code>c<sub>k</sub> = c<sub>0</sub> - 2 k</code> but it is much less obvious how to calculate <code>a<sub>k</sub></code> in general!

## Aside: Solving Degree 1, Linear, Non-Homogeneous Recurrence Relations

It turns out that this is an example of a well studied type of recurrence relation called a non-homogeneous linear recurrence relation of degree 1:

* Degree 1 means that <code>a<sub>k+1</sub> = f(a<sub>k</sub>)</code> (that is, it depends only on <code>a<sub>k</sub></code>, not say <code>a<sub>k-1</sub></code>, etc.).
* Linear means that `f` is a linear function. (In our case: `f(n) = 2n + 7`).
* Non-homogeneous means that `f(0) != 0`.

If only our recurrence was homogeneous it would be easy to solve. For example, consider the recurrence: <code>h<sub>0</sub> = 3</code> <code>h<sub>k+1</sub> = 2 h<sub>k</sub></code>. Here we can clearly see that <code>h<sub>k</sub> = 2<sup>k</sup> * 3</code>.

So, can we convert our recurrence relation into a homogeneous one? Let's try :) What if we consider the family of recurrence relations <code>H(n)<sub>k</sub> = a<sub>k</sub> + n</code> (parameterized by `n`) then:

* <code>H(n)<sub>k+1</sub></code> = <code>a<sub>k+1</sub> + n</code> = <code>(2 a<sub>k</sub> + 7) + n</code> = <code>2 (H(n)<sub>k</sub> - n) + 7 + n</code> = <code>2 H(n)<sub>k</sub> + (7 - n)</code>

so, then <code>H(7)<sub>k+1</sub> = 2 H(7)<sub>k</sub></code> is a homogeneous recurrence relation! Woohoo!

So <code>H(7)<sub>k</sub></code> = <code>2<sup>k</sup> * H(7)<sub>0</sub></code> = <code>2<sup>k</sup> * (a<sub>0</sub> + 7)</code>

And converting back into our original relation:

* <code>a<sub>k</sub></code> = <code>H(7)<sub>k</sub> - 7</code> = <code>2<sup>k</sup> * (a<sub>0</sub> + 7) - 7</code>

Note: For a more general approach: Someone at Columbia University posted [this excellent book chapter](http://www.cs.columbia.edu/~cs4205/files/CM2.pdf) explaining how to solve linear recurrences using generating functions which is really quite magical and mind-bending.

## Back to Repeating Rule 2

Ok, now that we have a closed-form solution to our new (Rule 2) recurrence relation:

* <code>a<sub>k</sub> = 2<sup>k</sup> * (a<sub>0</sub> + 7) - 7</code>

we have:

* Rule 2x: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>2k+r</sup> ...</code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>(2<sup>k</sup> * (a + 7) - 7)</sup> 1<sup>r</sup> ...</code>

How many steps does it take? Each iteration of Rule 2 takes <code>2 a<sub>k</sub><sup>2</sup> + 15 a<sub>k</sub> + 27</code> steps. So total it's:

$$ \sum_{i=0}^{k-1}(2 a_i^2 + 15a_i + 27) = \sum_{i=0}^{k-1}(2 (2^i (a + 7) - 7)^2 + 15 (2^i (a + 7) - 7) + 27) $$

$$ = \sum_{i=0}^{k-1}(2 (a + 7)^2 4^i + (15 + (2 \cdot 2 \cdot -7)) (a + 7) 2^i + (27 - 15 \cdot 7 + 2 \cdot 7^2)) $$

$$ = 2 (a + 7)^2 \sum_{i=0}^{k-1} 4^i - 13 (a + 7) \sum_{i=0}^{k-1} 2^i + 20 \sum_{i=0}^{k-1} 1 $$

$$ = 2 (a + 7)^2 \frac{4^k - 1}{3} - 13 (a + 7) (2^k - 1) + 20 k $$

Well that's quite a mouthfull! Let's give it a shorthand name:

$$ B(a, k) = \frac{2}{3} (a + 7)^2 (4^k - 1) - 13 (a + 7) (2^k - 1) + 20 k$$

## Remainder Behavior

Finally, once we've applied Rule 2x, we could be in one of two situations: `r = 0` or `r = 1`. Let's see what happens in each of those cases.

### Even

For `r = 0`:

* <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>0</sup> 0<sup>inf</sup></code> = <code>0<sup>inf</sup> <b><C</b> 0<sup>inf</sup></code> so we are on a blank tape and since `C0 -> 2RC`, we will never leave state `C` again. We've reached Chain Recurrence and the TM must have already Quasihalted.

But when did it quasihalt? For this we'll have to step back one iteration of Rule 2 and consider what happens starting at <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>2</sup> 0<sup>inf</sup></code>. Looking back inside the proof of Rule 2 above, we can see that the last non-`C` state the TM is in is `2a + 5` steps in, it is in state `B` (Note that Rule 1 never leaves state `C`). Therefore:

* <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>2</sup> 0<sup>inf</sup></code> -> Quasihalt at time `2a + 6` (Note: Quasihalting time is the step after the last step `B` is seen).

### Odd

For `r = 1`:

|     Step # |         Left tape |   State   | Right tape                  |
| ---------: | ----------------: | :-------: | :-------------------------- |
|          0 | 0<sup>inf</sup> | <b><C</b> | 0<sup>a</sup> 1 0<sup>inf</sup> |
|          1 | 0<sup>inf</sup> 2 | <b>C></b> | 0<sup>a</sup> 1 0<sup>inf</sup> |
|     a +  1 | 0<sup>inf</sup> 2<sup>a+1</sup> | <b>C></b> | 1 0<sup>inf</sup> |
|     a +  2 | 0<sup>inf</sup> 2<sup>a+2</sup> | <b>A></b> | 0<sup>inf</sup> |
|     a +  3 | 0<sup>inf</sup> 2<sup>a+2</sup> 1 | <b>B></b> | 0<sup>inf</sup> |
|     a +  4 | 0<sup>inf</sup> 2<sup>a+2</sup> 1 | <b><C</b> | 2 0<sup>inf</sup> |
|     a +  5 | 0<sup>inf</sup> 2<sup>a+3</sup> | <b>A></b> | 2 0<sup>inf</sup> |
|     a +  6 | 0<sup>inf</sup> 2<sup>a+3</sup> | <b><A</b> | 1 0<sup>inf</sup> |
|    2a +  9 | 0<sup>inf</sup> | <b><A</b> | 1<sup>a+4</sup> 0<sup>inf</sup> |
|    2a + 10 | 0<sup>inf</sup> 1 | <b>B></b> | 1<sup>a+4</sup> 0<sup>inf</sup> |
|    2a + 11 | 0<sup>inf</sup> 1 | <b><B</b> | 2 1<sup>a+3</sup> 0<sup>inf</sup> |
|    2a + 12 | 0<sup>inf</sup> | <b><B</b> | 2<sup>2</sup> 1<sup>a+3</sup> 0<sup>inf</sup> |
|    2a + 13 | 0<sup>inf</sup> | <b><C</b> | 2<sup>3</sup> 1<sup>a+3</sup> 0<sup>inf</sup> |
|    2a + 34 | 0<sup>inf</sup> | <b><C</b> | 0<sup>6</sup> 1<sup>a+3</sup> 0<sup>inf</sup> |

so we have our final rule:

* Rule 3: <code>0<sup>inf</sup> <b><C</b> 0<sup>a</sup> 1<sup>1</sup> 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>6</sup> 1<sup>a+3</sup> 0<sup>inf</sup></code> in `2a + 34` steps

## "Collatz-like Behavior"

Consider the tape configuration:

* <code>g(m) = 0<sup>inf</sup> <b><C</b> 0<sup>6</sup> 1<sup>m</sup> 0<sup>inf</sup>

Then:

* `g(2k+1)` = <code>0<sup>inf</sup> <b><C</b> 0<sup>6</sup> 1<sup>2k+1</sup> 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>(13 * 2<sup>k</sup> - 7)</sup> 1<sup>1</sup> 0<sup>inf</sup></code> in $$B(6, k)$$ steps by Rule 2x
* -> <code>0<sup>inf</sup> <b><C</b> 0<sup>6</sup> 1<sup>(13 * 2^k - 4)</sup> 0<sup>inf</sup></code> = <code>g(13 * 2^k - 4)</code> in $$2 (13 \cdot 2^k - 7) + 34 = 26 \cdot 2^k + 20$$ steps by Rule 3

and

* `g(2k)` = <code>0<sup>inf</sup> <b><C</b> 0<sup>6</sup> 1<sup>2k</sup> 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> <b><C</b> 0<sup>(13 * 2<sup>k-1</sup> - 7)</sup> 1<sup>2</sup> 0<sup>inf</sup></code> in $$B(6, k-1)$$ steps by Rule 2x
* -> Quasihalt at time $$2 (13 \cdot 2^{k-1} - 7) + 6 = 13 \cdot 2^k - 8$$.

Summarizing:
* `g(2k+1)` -> <code>g(13 * 2<sup>k</sup> - 4)</code> in $$B(6, k) + 26 * 2^k + 20$$
* `g(2k)` -> Quasihalt at time $$B(6, k-1) + 26 * 2{k-1} - 8$$

### Orbit from a blank tape

And finally, connecting this all together. Starting a blank tape, this TM reaches configuration `g(1)` at step 30, from which we get:

| Config |   k |  r | Steps Since Last                                        | Step At |
| -----: | --: | -: | :------------------------------------------------------ | :------ |
|   g(1) |   0 |  1 |                                                         | 30      |
|   g(9) |   4 |  1 | <code>B(6, 0) + 26 * 2<sup>0</sup> + 20</code> = 46     | 76      |
| g(204) | 102 |  0 | <code>B(6, 4) + 26 * 2<sup>4</sup> + 20</code> = 26_711 | 26_787  |
|  QHalt |     |    | <code>B(6, 101) + 13 * 2<sup>102</sup> - 8</code> = 724193411946051617510910916281388064798340875589283913992417982 | 724193411946051617510910916281388064798340875589283913992444770 |

### Is `g` Really "Collatz-like"?

`g` has behavior that is reminiscent of Collatz-like behavior. Specifically, it iterates checking the parity of it's argument at each iteration and branching based upon that parity. However, if you spend a few moments looking at the final recurrence for `g` you might notice something ... for all `k > 0`, <code>13 * 2<sup>k</sup> - 4</code> is even, therefore `g` is known to be "total". If a TM gets to any state `g(m)` it will Quasihalt in:
* 1 iteration if `m` is even
* 2 iterations if `m >= 3` is odd or
* 3 iterations in the one special case (`m = 1`)

### Similarity to pre-existing analyses

Note that this analysis is similar to [Pascal Michel's analysis](https://webusers.imj-prg.fr/~pascal.michel/beh.html#tm62h) of Pavel Kropitz's current `BB(6, 2)` record holder which similarly seems to have a 2 level recurrence behavior and thus leads to this sort of exponential recurrence (<code>C(9k + 9) -> C((50 * 4<sup>k+1</sup> - 11)/3)</code>). Sadly, unlike in that case, for our configuration `g` we will never have the possibility to iterate `g` more than 3 times and thus this machine cannot be modified in the way that "Wythagoras" did to create the current BB(7,2) champion.
