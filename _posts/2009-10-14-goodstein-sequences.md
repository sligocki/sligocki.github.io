---
layout: post
title: Goodstein Sequences
tags: big-number-notation
---

(This article was [original published](https://en.wikipedia.org/wiki/User:Sligocki/Goodstein_sequences) on in Oct 2009 on my Wikipedia User page. I'm re-posting on my blog to bring my writing together in one place.)

[Goodstein sequences](https://en.wikipedia.org/wiki/Goodstein%27s_theorem) are very long sequences which eventually reach 0, but run for so long that [Peano arithmetic](https://en.wikipedia.org/wiki/Peano_arithmetic) cannot be used to prove that they reach 0.

## Introduction
Let $$G(n)$$ be the Goodstein sequence starting with $$n$$ and ending at 0.

$$ G(3) = (2 + 1, 3, 3, 2, 1, 0) $$

$$ G(4) = (2^2, 2 \cdot 3^2 + 2 \cdot 3 + 2, \dots, 2\cdot 23^2, \dots, (24 \cdot 2^{24} - 1)^2, \dots, 3, 2, 1, 0) $$

Let $$g_n$$ be the base of the hereditary notation of the last term of $$G(n)$$ (Alternatively, it is the length of the Goodstein sequence + 1). We shall call these the Goodstein numbers.

| $$n$$ | $$g_n$$ |
| :---: | :-----: |
|   0   |    2    |
|   1   |    3    |
|   2   |    5    |
|   3   |    7    |
|   4   | $$3 \cdot 2^{402653211} - 1 = (24 \cdot 2^{24}) 2^{24 \cdot 2^{24}} - 1$$ |

Now, in fact, if $$ f(n) = (n+1) 2^{n+1} - 1 $$, then $$g_4 = f^3(2)$$. I show that this function has a meaning.

## Growth of Goodstein Numbers

Let us define a family of functions:
* $$ f_0(B) = B + 1 $$
* $$ f_k(B) = f_{k-1}^{B+1}(B) $$


Ah ha,
* $$ f_1(B) = 2B + 1 $$
* $$ f_2(B) = (B+1) \cdot 2^{B+1} - 1 $$
* $$ f_3(2) = f_2^3(2) = g_4 $$


In fact:

| $$ n $$ |  $$ g_n $$ | $$ G(n) $$ |
| ------: | ---------: | :--------- |
|       0 |          2 |            |
|       1 | $$f_0(2)$$ | $$(2^0, \dots)$$ |
|       2 | $$f_1(2)$$ | $$ (2^1, \dots) $$ |
|       3 | $$f_1(3)$$ | $$ (2^1 + 1, 3^1, \dots) $$ |
|       4 | $$f_3(2)$$ | $$ (2^2, 3^3 - 1, \dots) $$ |
|       5 | $$f_4(3)$$ | $$ (2^2 + 1, 3^3, 4^4 - 1, \dots) $$ |
|       6 | $$f_6(5)$$ | $$ (2^2 + 2, 3^3 + 2, 4^4 + 1, 5^5, 6^6 - 1, \dots) $$ |
|       7 | $$f_8(7)$$ | $$ (2^2 + 2 + 1, 3^3 + 3, 4^4 + 3, \dots, 7^7, 8^8 - 1, \dots) $$ |

Notice the pattern? If $$ B^k $$ appears in $$ G(n) $$ then $$ g_n = f_k(B) $$ (where $$B$$ is the base and $$k<B$$). Likewise, if $$ B^B $$ appears, then $$ g_n = f_{B+1}(B) $$.


In fact, let's rename our functions (here $$\omega$$ is a label, not a variable — in fact, it is actually an [ordinal number](https://en.wikipedia.org/wiki/Ordinal_number), or generalized index, who's properties I hope to take advantage of):
* $$ f_{\omega^0}(B) = B + 1 $$
* $$ f_{\omega^k}(B) = f_{\omega^{k-1}}^{B+1}(B) $$

And add a new one:
* $$ f_{\omega^\omega}(B) = f_{\omega^{B+1}}(B) $$

Thus, if we have a value of the form $$\alpha$$ at base $$B$$ in $$ G(n) $$, then $$ g_n = f_\alpha(B) $$.

## Derivation of Growth Function
> Note: It turns out that the function I define here is a variant of the [fast-growing hierarchy](https://en.wikipedia.org/wiki/Fast-growing_hierarchy) much like the [Hardy hierarchy](https://en.wikipedia.org/wiki/Hardy_hierarchy).

Goodstein talks about the hereditary form of a number and the unique ordinal number associated with each hereditary form. For example:

 * $$ 266 = 2^5 + 2^3 + 2 = 2^{2^2 + 1} + 2^{2 + 1} + 2 $$ is in form $$ \omega^{\omega^\omega + 1} + \omega^{\omega + 1} + \omega $$

Let us identify the hereditary form with that ordinal number.

If $$N$$ has hereditary form $$\alpha$$ with base $$B$$, then let $$ f_\alpha(B) $$ be the base at which the the Goodstein sequence starting at $$N$$ in base $$B$$ will end.

For some values of $$\alpha$$:
* $$ f_0(B) = B $$
* $$ f_1(B) = B + 1 $$
* $$ f_k(B) = f_1^k(B) = B + k $$


* $$ f_\omega(B) = f_B(B+1) = f_1^{B+1}(B) = 2B + 1 $$
* $$ f_{\omega \cdot k}(B) = f_\omega^k(B)) = (B + 1) 2^k - 1 $$


* $$ f_{\omega^2}(B) = f_{\omega \cdot B + B}(B+1) = f_{\omega \cdot B}(f_B(B+1)) = f_{\omega \cdot B}(f_\omega(B)) = f_{\omega \cdot (B+1)}(B) = f_\omega^{B+1}(B) = (B + 1) 2^{B + 1} - 1 $$
* $$ f_{\omega^2 \cdot k}(B) = f_{\omega^2}^k(B)) > 2 \uparrow \uparrow k $$


* $$ f_{\omega^3}(B) = f_{\omega^2 \cdot B + \omega \cdot B + B}(B+1) = f_{\omega^2 \cdot B}(f_{\omega^2 \cdot B + B}(B+1)) = f_{\omega^2 \cdot B}(f_{\omega^2}(B)) = f_{\omega^2 \cdot (B+1)}(B) = f_{\omega^2}^{B+1}(B) > 2 \uparrow \uparrow (B+1) $$


* $$ f_{\omega^k}(B) = f_{\omega^{k-1} \cdot (B+1)}(B) = f_{\omega^{k-1}}^{B+1}(B) > 2 \uparrow^{k-1} (B+1) $$


* $$ f_{\omega^\omega}(B) = f_{\omega^{B+1}}(B) > 2 \uparrow^B (B+1) = 2 \to B + 1 \to B $$
* $$ f_{\omega^\omega \cdot k}(B) = f_{\omega^\omega}^k(B) > 2 \to B + 1 \to k \to 2 $$


Note: $$ h(n) = 3 \uparrow^n 3 < 2 \uparrow^n (n-1) = f_{\omega^\omega}(n-1) $$. So [Graham's number](https://en.wikipedia.org/wiki/Graham%27s_number) $$ \mathcal{G} = h^{64}(4) < f_{\omega^\omega}^{64}(4) = f_{\omega^\omega \cdot 64}(4) $$.

Now $$ G(12) = (2^{2+1} + 2^2, \dots, g_4^{g_4 + 1}, g_4 (g_4 + 1)^{g_4 + 1}, \dots) $$ where we remember that $$ g_4 = f_{\omega^\omega}(2) = 3 \cdot 2^{402653211} - 1 > 64 $$. So, $$g_{12} = f_{\omega^\omega \cdot g_4}(g_4 + 1) > f_{\omega^\omega \cdot 64}(4) > \mathcal{G} $$


* $$ f_{\omega^{\omega + 1}}(B) = f_{\omega^\omega \cdot B + \omega^B \cdot B + \cdots + B}(B+1) = f_{\omega^\omega \cdot B}(f_{\omega^{B+1}}(B)) = f_{\omega^\omega \cdot B}(f_{\omega^\omega}(B)) = f_{\omega^\omega \cdot (B + 1)}(B) = f_{\omega^\omega}^{B+1}(B) > 2 \to B + 1 \to B + 1 \to 2 $$
* $$ f_{\omega^{\omega + 1} \cdot k}(B) = f_{\omega^{\omega + 1}}^k(B) > 2 \to B + 1 \to k \to 3 $$


* $$ f_{\omega^{\omega + 2}}(B) = f_{\omega^{\omega + 1} \cdot B + \omega^\omega \cdot B + \omega^B \cdot B + \cdots + B}(B+1) = f_{\omega^{\omega + 1} \cdot B}(f_{\omega^{\omega+1}}(B)) = f_{\omega^{\omega + 1} \cdot (B + 1)}(B) = f_{\omega^{\omega + 1}}^{B+1}(B) > 2 \to B + 1 \to B + 1 \to 3 $$


* $$ f_{\omega^{\omega + k}}(B) = f_{\omega^{\omega + (k-1)} \cdot (B + 1)}(B) = f_{\omega^{\omega + (k-1)}}^{B+1}(B) > 2 \to B + 1 \to B + 1 \to k + 1 $$


* $$ f_{\omega^{\omega \cdot 2}}(B) = f_{\omega^{\omega + (B+1)}}(B) > 2 \to B + 1 \to B + 1 \to B + 2 $$

...

* $$ f_{\omega^{\omega^\omega}}(B) = f_{\omega^{\omega^{B+1}}}(B) >> 2 \to \overbrace{B + 1 \to \cdots \to B + 1}^{B^2 + 2} $$


Now let's look back at the table:

| $$ n $$ | $$ g_n $$ | $$ G(n) $$ |
| -: | :- | :- |
|  0  |  $$ f_0(2) = 2 $$  |  $$ (0, \dots) $$
|  1  |  $$ f_1(2) = f_1(g_0) = 3 $$  |  $$ (1, \dots) $$
|  2  |  $$ f_\omega(2) = f_\omega(g_0) = 5 $$  |  $$ (2^1, \dots) $$
|  3  |  $$ f_{\omega+1}(2) = f_\omega(f_1(2)) = f_\omega(g_1) = 7 $$ |  $$ (2^1 + 1, 3^1, \dots) $$
|  4  |  $$ f_{\omega^\omega}(2) = f_{\omega^\omega}(g_0) $$ |  $$ (2^2, \dots) $$
|  5  |  $$ f_{\omega^\omega + 1}(2) = f_{\omega^\omega}(f_1(2)) = f_{\omega^\omega}(g_1) $$ |  $$ (2^2 + 1, 3^3, \dots) $$
|  6  |  $$ f_{\omega^\omega + \omega}(2) = f_{\omega^\omega}(f_\omega(2)) = f_{\omega^\omega}(g_2) $$ |  $$ (2^2 + 2, 5^5, \dots) $$
|  7  |  $$ f_{\omega^\omega + \omega + 1}(2) = f_{\omega^\omega}(f_\omega(f_1(2))) = f_{\omega^\omega}(g_3) $$ |  $$ (2^2 + 2 + 1, 3^3 + 3, \dots, 7^7, \dots) $$
|  8  |  $$ f_{\omega^{\omega + 1}}(2) = f_{\omega^{\omega + 1}}(g_0) = f_{\omega^\omega}^{g_0 + 1}(2) = f_{\omega^\omega}^{g_0}(g_4) $$ |  $$ (2^{2 + 1}, \dots, 2 \cdot g_4^{g_4}, \dots) $$
|  9  |  $$ f_{\omega^{\omega + 1} + 1}(2) = f_{\omega^{\omega + 1}}(f_1(2)) = f_{\omega^{\omega + 1}}(g_1) = f_{\omega^\omega}^{g_1 + 1}(f_1(2)) = f_{\omega^\omega}^{g_1}(g_5) $$ |  $$ (2^{2 + 1} + 1, 3^{3 + 1}, \dots, 3 \cdot g_5^{g_5}, \dots) $$
| 10  |  $$ f_{\omega^{\omega + 1} + \omega}(2) = f_{\omega^{\omega + 1}}(f_\omega(2)) = f_{\omega^{\omega + 1}}(g_2) = f_{\omega^\omega}^{g_2 + 1}(g_2) = f_{\omega^\omega}^{g_2}(g_6) $$ |  $$ (2^{2 + 1} + 2, \dots, 5^{5 + 1}, \dots, 5 \cdot g_6^{g_6}, \dots) $$
| 11  |  $$ f_{\omega^{\omega + 1} + \omega + 1}(2) = f_{\omega^{\omega + 1}}(f_\omega(f_1(2))) = f_{\omega^{\omega + 1}}(g_3) $$ |  $$ (2^{2 + 1} + 2 + 1, 3^{3 + 1} + 3, \dots, 7^{7 + 1}, \dots, 7 \cdot g_7^{g_7}, \dots) $$
| 12  |  $$ f_{\omega^{\omega + 1} + \omega^\omega}(2) = f_{\omega^{\omega + 1}}(f_{\omega^\omega}(2))) = f_{\omega^{\omega + 1}}(g_4) $$ |  $$ (2^{2 + 1} + 2^2, \dots, g_4^{g_4 + 1}, \dots, g_4 \cdot g_8^{g_8}, \dots) $$
| 13  |  $$ f_{\omega^{\omega + 1} + \omega^\omega + 1}(2) = f_{\omega^{\omega + 1}}(f_{\omega^\omega}(f_1(2)))) = f_{\omega^{\omega + 1}}(g_5) $$ |  $$ (2^{2 + 1} + 2^2 + 1, 3^{3+1} + 3^3, \dots, g_5^{g_5 + 1}, \dots, g_5 \cdot g_9^{g_9}, \dots) $$
| 14  |  $$ f_{\omega^{\omega + 1} + \omega^\omega + \omega}(2) = f_{\omega^{\omega + 1}}(f_{\omega^\omega}(f_\omega(2)))) = f_{\omega^{\omega + 1}}(g_6) $$ |  $$ (2^{2 + 1} + 2^2 + 2, \dots, 5^{5+1} + 5^5, \dots, g_6^{g_6 + 1}, \dots, g_6 \cdot g_{10}^{g_{10}}, \dots) $$
| 15  |  $$ f_{\omega^{\omega + 1} + \omega^\omega + \omega + 1}(2) = f_{\omega^{\omega + 1}}(f_{\omega^\omega}(f_\omega(f_1(2))))) = f_{\omega^{\omega + 1}}(g_7) $$ |  $$ (2^{2 + 1} + 2^2 + 2 + 1, \dots, 7^{7+1} + 7^7, \dots, g_7^{g_7 + 1}, \dots, g_7 \cdot g_{11}^{g_{11}}, \dots) $$
| 16  |  $$ f_{\omega^{\omega^\omega}}(2) = f_{\omega^{\omega^\omega}}(g_0) = ? $$ | $$ (2^{2^2}, \dots, 2 \cdot n^{2 \cdot n^2 + 2 \cdot n + 2}, \dots) $$


## Some Examples

* $$ g_{16} = f_{\omega^{\omega^\omega}}(g_0)
 = f_{\omega^{\omega^{g_0+1}}}(g_0)
 = f_{\omega^{\omega^{g_0} \cdot (g_0 + 1)}}(g_0)
 = f_{\omega^{\omega^{g_0} \cdot g_0 + g_0 + 1}}(g_0)
 = f_{\omega^{\omega^{g_0} \cdot g_0 + g_0}}^{g_0 + 1}(g_0)
 = f_{\omega^{\omega^{g_0} \cdot g_0 + g_0}}^{g_0}(f_{\omega^{\omega^2 \cdot 2 + 2}}(g_0))
 = ?
 $$

* $$ \begin{array}{rcl}
g_6 &=& f_{\omega^\omega + \omega}(2) = f_{\omega^\omega}(f_\omega(2)) \\
    &=& f_{\omega^\omega}(5) = f_{\omega^6}(5) = f_{\omega^5}^6(5) \\
    &=& f_{\omega^5}^5(f_{\omega^4}^5(f_{\omega^3}^5(f_{\omega^2}^5(f_{\omega^2}(5))))) \\
    &=& f_{\omega^5}^5(f_{\omega^4}^5(f_{\omega^3}^5(f_{\omega^2}^5(2^{B+1} - 1)))) \\
    &=& f_{\omega^5}^5(f_{\omega^4}^5(f_{\omega^3}^5(f_{\omega^2}^4(2^{2^{B+1} - 1} - 1)))) \\
    &=& f_{\omega^5}^5(f_{\omega^4}^5(f_{\omega^3}^5(f_{\omega^2}^3(2^{2^{2^{B+1} - 1} - 1} - 1)))) \\
\end{array} $$
