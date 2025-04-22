---
layout: post-custom
title: Modular Tetration is Easy
tags: busy-beaver
---

Is there an efficient way to compute the remainder of $$a \uparrow\uparrow b$$ modulo $$m$$ for very large $$b$$? At first, I thought the best algorithm might be $$O(b)$$ which would basically make it impossible for some number like $$10 \uparrow\uparrow 10 \uparrow\uparrow 10$$. But it turns out to be more like $$O(m)$$. Specifically, after some point, adding more height to the tetration tower has no effect on the remainder:

$$ \forall a \in \mathbb{N} \; \forall n \ge m: \;\; a \uparrow\uparrow n \equiv a \uparrow\uparrow m \pmod{m} $$

In fact, more generally, once you get $$m$$ layers of exponential tower, whatever is above that is irrelevant:

$$ \forall a, x, y, m \in \mathbb{N}: \;\; (a \uparrow)^m \; x \equiv (a \uparrow)^m \; y \pmod{m} $$


## Modular Exponentiation

Tetration is just repeated exponentiation, so before we dig into modular tetration, we need to understand modular exponentiation.

How do you efficiently compute $$a^b \pmod{m}$$ for extremely large exponents $$b$$?

It turns out that if $$k$$ is the maximum exponent in the prime factorization of $$m$$:[^cycle-length]

$$ a^{k + \lambda(m)} \equiv a^k \pmod{m} $$

where $$\lambda(m)$$ is the [Carmichael function](https://en.wikipedia.org/wiki/Carmichael_function).

[^cycle-length]: See the ["Exponential cycle length"](https://en.wikipedia.org/wiki/Carmichael_function#Exponential_cycle_length) section from Wikipedia's "Carmichael function" article.

Furthermore, for any $$r \ge k$$ and $$n \ge 0$$:

$$ a^{r + n \lambda(m)} \equiv a^r \pmod{m} $$

Let's define

$$ s_m(b) = \min \{ r : k \le r \le b, b \equiv r \pmod{\lambda(m)} \} $$

(or $$s_m(b) = b$$ if $$b < k$$)

it follows that

$$ a^b \equiv a^{s_m(b)} \pmod{m} $$

$$ s_m(b) \equiv b \pmod{\lambda(m)} $$

$$ k \le s_m(b) < k + \lambda(m) $$

So, we have reduced to problem of computing $$a^b \pmod{m}$$ to computing $$s_m(b)$$ and $$a^{s_m(b)} \pmod{m}$$ where computing $$s_m(b)$$ is roughly computing $$b \pmod{\lambda(m)}$$ and computing $$a^{s_m(b)} \pmod{m}$$ is considered easy since $$s_m(b) < k + \lambda(m)$$.


## Repeated Modular Exponentiation

Now let's consider repeated exponentiation:

$$ (a \uparrow)^n x \pmod{m} $$

for sufficiently large $$n$$ ($$n > m$$). Applying the previous section, we can see that:

$$ (a \uparrow)^n x = a^{(a \uparrow)^{n-1} x} \equiv a^{s_m((a \uparrow)^{n-1} x)} \pmod{m} $$

where

$$ s_m((a \uparrow)^{n-1} x) = s_m(a^{})


Now, applying this logic to tetration, we can see that computing

$$ a \uparrow\uparrow b \pmod{m} $$

reduces to computing

$$ a \uparrow\uparrow (b-1) \pmod{\lambda(m)} $$

which reduces to computing

$$ a \uparrow\uparrow (b-2) \pmod{\lambda(\lambda(m))} $$

...

$$ a \uparrow\uparrow (b-n) \pmod{\lambda^n(m)} $$


### Repeating Carmichael

Before going further, we should consider the behavior of iterating the Carmichael function $$\lambda^k(m)$$. The exact behavior is non-trivial, but we will take advantage of one simple fact:

$$ \forall m \ge 2: 1 \le \lambda(m) < m $$

in other words, the sequence

$$ m, \lambda(m), \lambda^2(m), ..., \lambda^n(m), ... $$

is strictly decreasing until it reaches 1. We can define

$$ h(m) = \min \{ n : \lambda^n(m) = 1 \} $$

it is easy to see that

$$ h(m) < m $$

Blakley and Borosh (1983)[^blakley1983] prove a better bound:

$$ h(m) \le \lceil \log_2(m) \rceil $$

[^blakley1983]: G.R. Blakley, I. Borosh. Modular arithmetic of iterated powers. Computers & Mathematics with Applications. Volume 9. Issue 4. 1983. Pages 567-581. ISSN 0898-1221. <https://doi.org/10.1016/0898-1221(83)90114-1>.


### Repeated Modular Exponentiation




## Footnotes