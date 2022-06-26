---
layout: post
title: Extending Up-arrow Notation
tags: big-number-notation
---

[Knuth's up-arrow notation](https://en.wikipedia.org/wiki/Knuth%27s_up-arrow_notation) is a great way to express large numbers. Naturally, it works especially well for numbers produced by repeated exponentiation, such as the [recent BB(6, 2) champion]({% post_url 2022-06-21-bb-6-2-t15 %}) which runs for > 10↑↑15 steps. In that article I prove that a TM runs for a number of steps $$N$$ with $$10↑↑15 < N < 10↑↑16$$, so using this simple up-arrow notation, that is as precise as we can get to defining the size of $$N$$.

This is sort of the tetration[^tetration] equivalent of saying $$10^{15} < M < 10^{16}$$. In other words, it tells us the "order of magnitude" without any further precision. For most Busy Beaver results, I generally consider this to be a sufficient level of detail. For example, in my [BB(6, 2) > 10↑↑15]({% post_url 2022-06-21-bb-6-2-t15 %}) article, I list all the BB(6, 2) bounds in this loose way.

However, using standard [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation), we can write $$M = 8.13 \times 10^{15}$$ which provides more precision by including a _significand_ (the coefficient $$8.13$$). Among other things, this allows us to compare two numbers which are the same order of magnitude. Say $$1.38 \times 10^{15} < 8.13 \times 10^{15}$$.

Is there an analogous way to add some sort of _hyper-significand_ to up-arrow notation? Let's deconstruct this notation a bit: one way to think about scientific notation is that we start with a number $$M$$ and repeatedly divide it by 10 until we get a value in the range $$[1, 10)$$. That final value is the significand and the number of iterations is the exponent. Moving up to tetration, we could say that we start with a number $$N$$ and repeatedly take the logarithm (base 10) of the number until we get a value in the range $$[1, 10)$$. Let's say that we took the logarithm 5 times and ended up with a value 4.31, that would be equivalent to the number:

$$ N = 10^{10^{10^{10^{10^{4.31}}}}} $$

But, unfortunately, there is no way to write this number succinctly using standard up-arrow notation. For example, it might seem natural to write it as $$10↑↑5↑4.31$$ because this sort of describes the way it's written (a tower of 5 10s topped by a 4.31). However, this notation is extremely ambiguous. Two reasonable ways to parse it would be:

$$ 10↑↑(5↑4.31) > 10↑↑1029 \gg 10↑↑6 > N $$

$$ (10↑↑5)↑4.31 = 10^{4.31 \times 10↑↑4} < 10^{10^{1 + 10^{10^{10}}}} < N $$

Unlike with scientific notation, there is no way to start with $$10↑↑5$$ and produce $$N$$ (because exponentiation is **not** commutative). Instead, if we want to have such a notation, we will have to extend the notation (or come up with new notation).


## Extended Up-arrow Notation

With that motivation, I propose we adopt some sort of extended up-arrow notation to represent:

$$ \underbrace{a^{a^{~\cdot~^{~\cdot~^{~\cdot~^{a^{c}}}}}}}_{n \mbox{ copies of } a} $$

I don't know what a good notation would be. But to get the conversation started I will suggest:

$$ a↑↑n[↑c] $$

Recursively defined by:

 1. $$ a↑↑0[↑c] = c $$
 2. $$ a↑↑(n+1)[↑c] = a^{a↑↑n[↑c]} $$

Note: this notation can be (analogously) extended to higher levels of up-arrow notation, like:

$$ a↑↑↑n[↑↑c] $$

$$ a↑↑↑↑n[↑↑↑c] $$

$$ a↑^{k+1}n[↑^{k}c] $$


## Existing Notation

On the [Wikipedia tetration page](https://en.wikipedia.org/wiki/Tetration#Notation) they list 2 other pre-existing notations for this concept:

$$ exp_a^n(c) $$

$$ (a↑)^n(c) $$

Both of which express the notion of iterating the function $$ x \mapsto a^x $$, $$n$$ times starting from $$c$$ which is exactly equal to my proposed $$ a↑↑n[↑c] $$.

It would be nice to use a pre-existing notation rather than designing a new one, however, these both seem like they will be horrible to write in ASCII. IMHO, writing `exp_a^n(c)` or `(a^)^n(c)` both look very confusing compared to `a^^n[^c]`.

Another option would be to use the [fast growing hierarchy](https://en.wikipedia.org/wiki/Fast-growing_hierarchy) of functions. In this system $$f_2(n) = 2^n \cdot n > 2^n$$ and $$f_3(n) = f_2^n(n) > 2↑↑n[↑n] > 2↑↑n$$ So, we could write $$f_2^n(c)$$ to mean _roughly_ $$2↑↑n[↑c]$$.


## Hyper Scientific Notation

In addition, I propose a standard convention for representing numbers so that they can be easily compared. We could call it _hyper_ scientific notation. Specifically, as a parallel to normal scientific notation: always convert to $$a = 10$$ and $$ 1 \le c < 10$$. So, for example:

$$ 10↑↑5[↑4.31] $$

Now, we can compare numbers just like in scientific notation:

$$ n < m \to 10↑↑n[↑c] < 10↑↑m[↑d] $$

for any $$c, d \in [1, 10)$$

$$ c < d \to 10↑↑n[↑c] < 10↑↑n[↑d] $$

and (for $$1 < c < 10$$):

$$ 10↑↑n < 10↑↑n[↑c] < 10↑↑(n+1) $$


## Fractional Heights

Instead of writing $$8.13 \times 10^{15}$$, another common notation is to write $$10^{15.91}$$. Is there an analogue in tetration? As far as I can tell, there is no standard definition for $$a↑↑x$$ for non-integer $$x$$. However, Wikipedia does [list one extension](https://en.wikipedia.org/wiki/Tetration#Linear_approximation_for_real_heights) to real number heights that appears to have some useful properties. Specifically, they define:

$$
a↑↑x = \begin{cases}
  a^x & 0 \le x < 1 \\
  a^{a↑↑(x-1)} & x \ge 1
\end{cases}
$$

Converting between this form and hyper scientific notation are just as straightforward as in the exponential case:

Assuming that $$1 \le c < a$$ (so that $$0 \le \log_{a}(c) < 1$$), then:

$$ a↑↑n[↑c] = a↑↑(n + \log_{a}(c)) $$

and if $$n = \lfloor x \rfloor$$ is the integer part of $$x$$ and $$f = x - n$$ is the fractional part of $$x$$, then:

$$ a↑↑x = a↑↑n[↑a^f] $$

So, our example:

$$ 10↑↑5[↑4.31] = 10↑↑5.63 $$

This notation has the advantage that (A) it does not require new syntax, (B) it is less "busy" or "cluttered" than the hyper scientific notation and (C) comparing two numbers in this notation is even easier than before (there's only one parameter to compare!).

However, it has the disadvantage that (1) this extension to the real numbers is not universally accepted, (2) while this extension to the reals is continuous, it is not differentiable at integer points, (3) it is (perhaps) harder to explain the new notation to someone who hasn't learned about it (I think it's a bit easier conceptually to explain the idea of a stack of $$n$$ $$a$$s with a $$c$$ on the top) and (4) AFAIK, there is no extension to higher up-arrow functions to real numbers.


## Improving BB(6, 2) Bound

Using these new notations, we can now improve the precision of the 10↑↑15 bound for Pavel's TM. Specifically, starting from this result from the [BB(6, 2) > 10↑↑15]({% post_url 2022-06-21-bb-6-2-t15 %}#standardizing-notation) article:

$$ k_{n+1} / T - S > 10^{k_n / T - S} $$

and using $$k_2 = 22143$$:

$$k_2 / T - S > 10564.5$$

$$ k_n / T - S > 10↑↑(n-2)[↑10564.5] $$

$$ BB(6, 2) \ge s(\mbox{t15}) > A_{16} > 10^{k_{15} / T} > 10^{10↑↑13[↑10564.5]} > 10↑↑15[↑4.0238] > 10↑↑15.60463 $$

### Upper Bound

For the upper bound, it will help to compute a slightly tighter relation:

$$ s(\mbox{t15}) < A_{16}^2 < 3^{2 k_{15} + 6} < 10^{k_{15} + 3} $$

$$ \frac{k_{n+1} + 3}{T} < k_{n+1} + 3 < 3^{k_n + 3} = 10^{\frac{k_n + 3}{T}} $$

and using $$k_2 = 22143$$:

$$ \frac{k_2 + 3}{T} < 10566.4 $$

$$ k_n + 3 < 10↑↑(n-2)[↑10566.4] $$

$$ s(\mbox{t15}) < 10^{10↑↑(13)[↑10566.4]} < 10↑↑15[↑4.0240] < 10↑↑15.60466 $$

So the full precision is:

$$ 10↑↑15[↑4.0238] < \sigma(\mbox{t15}) < s(\mbox{t15}) < 10↑↑15[↑4.0240] $$

$$ 10↑↑15.60463 < \sigma(\mbox{t15}) < s(\mbox{t15}) < 10↑↑15.60466 $$

These bounds can probably be made more precise by starting from $$k_3 = \frac{3^{22146} - 17}{8}$$.


## Footnotes

[^tetration]: [Tetration](https://en.wikipedia.org/wiki/Tetration) is another name for repeated exponentiation, aka ↑↑.
