---
layout: post
title: Up-arrow Properties
tags: big-number-notation
---

(This article was [original published](https://en.wikipedia.org/wiki/User:Sligocki/up-arrow_properties) on in Oct 2009 on my Wikipedia User page. I'm re-posting on my blog to bring my writing together in one place.)

Some useful definitions and properties for [Knuth's up-arrow notation](https://en.wikipedia.org/wiki/Knuth%27s_up-arrow_notation)


## Definition

$$a \uparrow^n b$$ is defined for a, b and n are integers $$n \ge 1$$ and $$b \ge 0$$.

 1. $$ a \uparrow^1 b = a^b $$
 2. $$ a \uparrow^n 0 = 1 $$
 3. $$ a \uparrow^n b = a \uparrow^{n-1} (a \uparrow^n (b-1)) $$

Therefore $$a\uparrow^n b = a \uparrow^{n-1} a \uparrow^{n-1} \cdots \uparrow^{n-1} a$$ (with b copies of a, where $$\uparrow$$ is right associative) and so it is seen as an extension of the series of operations $$+, \times, \uparrow, \uparrow^2, \dots$$ where $$\uparrow$$ is basic exponentiation


## Basic Properties

 1. $$ a \uparrow^n 1 = a $$
 2. $$ 2 \uparrow^n 2 = 2^2 = 2 \times 2 = 2 + 2 = 4 $$


## Extension
We can extend the uparrows to include multiplication and addition as the [[hyper operator]].

This system may be consistently expanded to include multiplication, addition and incrementing:

 1. $$ a \uparrow^{-2} b = b+1 $$
 2. $$ a \uparrow^{-1} b = a+b $$
 3. $$ a \uparrow^0 b = ab $$
 4. $$ a \uparrow^1 b = a^b $$
 5. $$ a \uparrow^n 0 = 1, \forall n \ge 1 $$
 6. $$ a \uparrow^n b = a \uparrow^{n-1} (a \uparrow^n (b-1)) $$


### Proof of consistency by induction.
We will show that Rules 3, 5 and 6 imply rule 4
Assume that $$ a \uparrow^1 \beta = a^\beta $$ for any $$ \beta < b $$, then

 * $$ a \uparrow^1 b = a \uparrow^0 (a \uparrow^1 (b-1)) = a \cdot a^{b-1} = a^b $$ by rule 6, rule 3 and assumption

Furthermore, $$ a \uparrow^1 0 = 1 = a^0 $$ by rule 5

Thus the assumption is true for all $$\beta \ge 0 $$

Likewise we can show that Rules 2, 5, 6 imply Rule 3 and that Rules 1, 5, 6 imply Rule 2.
Therefore, Rules 1, 5, 6 imply Rules 4, 5, 6 and so consistently extend the system.

**QED**

Clearly some of the properties do not extend.


## Changing Bases
Todo: How do you change bases.

Example:
* $$ 3 \uparrow^k n \approx 10 \uparrow^k n^\prime $$ what is $$n^\prime$$ ?

For $$k = 1$$:

$$ a \uparrow n = a^n = b^{n\log_b(a)} = b \uparrow (\log_b(a) n) $$

For $$k = 2$$: For all $$a < b$$ there is a unique $$\delta$$ such that

$$ b \uparrow \uparrow (n - \delta - 1) < a \uparrow \uparrow n < b \uparrow \uparrow (n - \delta) $$

for all sufficiently large n

Examples:
* $$ 10 \uparrow \uparrow (n-1) < 3 \uparrow \uparrow n < 10 \uparrow \uparrow n $$ for all $$ n \ge 3 $$
* $$ 10 \uparrow \uparrow (n-3) < 2 \uparrow \uparrow n < 10 \uparrow \uparrow (n-2) $$ for all $$ n \ge 4 $$

Thus the base of a tetration is not very important, they all grow at approximately the same rate eventually. (See [Notes](#notes))

In fact these numbers $$\delta$$ grow very slowly.

Claim:

$$ a \uparrow \uparrow (n+k-1) < (a \uparrow \uparrow k) \uparrow \uparrow n < a \uparrow \uparrow (n+k) \,$$

Note, the left inequality is easy to prove:

$$ (a \uparrow \uparrow k) \uparrow \uparrow n = ((a \uparrow \uparrow k) \uparrow)^{n-1} (a \uparrow \uparrow k) > (a \uparrow)^{n-1} (a \uparrow \uparrow k) = a \uparrow \uparrow (n+k-1) \,$$

Claim:

$$ n \uparrow^n n < (3 \uparrow^n 3) \uparrow^n (3 \uparrow^n 3 - 3) < 3 \uparrow^n (3 \uparrow^n 3) = 3 \uparrow^{n+1} 3 \,$$


## Notes

Note, for any $$a,b$$, $$\delta$$ is uniquely defined, let's call it $$\delta_{ab}$$. Making a small table we get:

| $$ _a \backslash ^b $$ | 2 | 3 | 4 | 5 | 6 | ... | 10 | 100 | 1000 | 10000 |
| ---------------------: |-: |-: |-: |-: |-: | :-: | -: | --: | ---: | ----: |
|                      2 |   | 2 | 2 | 2 | 3 | ... |  3 |   3 |    3 |     4 |
|                      3 |   |   | 1 | 1 | 1 | ... |  1 |   2 |    2 |     2 |

As you can see, these values rise very slowly, in fact, (I believe that) if $$ b = a \uparrow \uparrow k $$, then $$ \delta_{ab} = k $$. The following table shows the smallest number $$b$$ such that $$ \delta_{ab} = \delta $$

| $$ _\delta \backslash ^a $$ | 2 | 3 | 4 | 5 | 6 | 7 | 8 | ... | 10 | 100 |
| -----: | ---: | -: | -: | --: | ---: | -----: | ------: | :-: | -: | --: |
|      1 |      |  4 |  5 |   6 |    7 |      8 |       9 | ... | 11 | 101 |
|      2 |    3 | 12 | 81 | 759 | 9164 | 135608 | 2376342 | ... |    |     |
|      3 |    6 |    |    |     |      |        |         | ... |    |     |
|      4 | 5298 |    |    |     |      |        |         | ... |    |     |
