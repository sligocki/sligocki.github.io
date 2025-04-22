---
layout: post-custom
title: Modular Exponentiation
tags: busy-beaver
---

In [BB(6, 2) > 10↑↑15]({% post_url 2022-06-21-bb-6-2-t15 %}) I described a little bit about the math required to compute $$b^n \pmod{m}$$ even when $$n$$ is so large that we cannot compute $$b^n$$ directly. But in order to implement this for general $$b, n, m$$ requires a bit more detail. I describe the general algorithm here.

Note: Throughout this article I will use the notation $$a \;\%\; m$$ to mean the remainder of $$a$$ when divided by $$m$$. In other words

$$a \;\%\; m = r  \iff  0 \le r < m \text{ and } a \equiv r \pmod{m} $$


## Binary and Formula Numbers

In this article it will be helpful to distinguish between two types of numbers: "binary" and "formula".

* Binary numbers are simply ones which are stored directly in memory in binary notation. This could be in either a fixed size integer representation (like `int64`) or an arbitrary sized "bigint" representation (like Python `int`).
* Formula numbers are numbers represented by some sort of formula or alternative notation. All of the recent "tetration-level" or "pentation-level" champion scores are formula numbers.

The notable thing here is that for large enough numbers they can only be represented as formula numbers and at that point most classic algorithms break down.


## Scope

Efficient modular exponentiation has a long history in Computer Science. The Wikipedia [Modular Exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation) has a good overview of the typical CS algorithms which are optimized for the case when $$b^n$$ is a formula number, but $$b, n, m$$ are all binary numbers. Many programming languages already have efficient functions for computing in this case (for example, python's 3 argument `pow(b, n, m)` function).

We are taking things one step further, where the exponent $$n$$ is also allowed to be a formula number. Specifically, ones where $$n$$ is represented by a recursive formula which itself will have exponentials. But note that in our use-case $$m$$ will still be a binary number.

Examples of these numbers are the intermediate block repetitions counts needed for simulating "tetration-level" TMs such as the TMs which demonstrated that BB(6, 2) > 10↑↑15, [BB(2, 6) > 10↑↑70]({% post_url 2023-04-24-bb-2-6-t70 %}), [BB(3, 4) > 10↑↑2048]({% post_url 2023-05-17-bb-3-4-t2048 %}).

This discussion will not apply to "pentation-level" TMs like [BB(2, 6) > 10↑↑10↑↑10↑↑3]({% post_url 2023-05-20-bb-2-6-p3 %}) which require more complex formulas to express (using arbitrary repetitions of an exponential function).


## Naive algorithm

A naive algorithm for computing $$b^n \;\%\; m$$ is to compute the sequence

$$ a_0 = 1 \;\; a_k = (a_{k-1} \cdot b) \% m $$

then

$$ b^n \;\%\; m = a_n $$

This is clearly better than computing $$b^n$$ directly and then taking the remainder, but is still $$O(n)$$ so it will be completely impractical for a situation like all of the recent "tetration-level" machines where, say $$n > 10↑↑14$$!


## Modular Cycles

However, reasoning about the sequence $$(a_k)$$ above is quite useful. Notice, for example, that all $$0 \le a_k < m$$, so, by the pigeon-hole principle, there must exist $$0 \le q < r \le m$$ such that $$a_q = a_r$$. Then, since $$a_k$$ depends only on $$a_{k-1}$$ we can see that for all $$k \ge 0$$, $$a_{q+k} = a_{r+k}$$. Or if we let $$d = r - q$$, then for any $$k \ge q$$ and any $$t \ge 0$$

$$ a_{k + td} = a_k $$

In other words, the sequence $$(a_k)$$ is eventually cyclic with a period and start point both $$\le m$$.

So, now we have simplified the problem to:

1. Finding such a $$q, r$$ for this sequence,
2. Finding a $$k$$ such that $$q \le k < r$$ and $$k \equiv n \pmod{d}$$ and
3. Computing $$a_k$$ directly

For part 3: Since $$k \le 2m$$ is a binary number, we will simply compute $$a_k = b^k \;\%\; m$$ directly using existing efficient algorithms (as described in the Wikipedia article).

For part 2: $$k = (n-q)\%d + q$$, so if $$n$$ is still a formula number, this will require recursively applying this modular exponentiation algorithm.

For part 1: We'll describe some strategies below.


### Direct Search

The simplest method for finding such a $$q, r$$ above would be to simply directly compute $$(a_k)$$ until we find a repeat. Since we know there exist $$q < r \le m$$ this is $$O(m)$$ in both time and space. This method has the additional advantage that it will find the smallest such $$q, r$$ which will make the subsequent calculations easier.

For small enough $$m$$, this is a fine algorithm and it is enough to prove the "BB(6, 2) > 10↑↑15" result which only requires computing with a maximum of $$m = 2^{15}$$. However, as $$m$$ grows this will become very inefficient. It has no hope of proving the [BB(2, 6) > 10↑↑91](https://groups.google.com/g/busy-beaver-discuss/c/QBC_0X5Re3I/m/0E7EazrMAgAJ) bound, for example, which requires computing with a maximum of $$m = 2 \cdot 3^{90} > 10^{43}$$ in the final iteration.[^bb26]

[^bb26]: Incidentally, this is exactly the reason that I did not find this BB(2, 6) > 10↑↑91 TM in my initial search and instead reported on the BB(2, 6) > 10↑↑70 bound (which happened to not require computing modular exponentiation). I was using this direct search at the time. Pavel was using the Carmichael Function and so was able to find the 10↑↑91 TM a few days later.


### Carmichael Function

When $$m$$ is too large to search directly, there is a more clever mathematical way to find such a $$q, r$$.

The [Carmichael function](https://en.wikipedia.org/wiki/Carmichael_function) $$\lambda(m)$$ is defined as the smallest exponent $$n$$ such that $$b^n \equiv 1 \pmod{m}$$ for all $$b$$ coprime to $$m$$. This function also turns out to be efficient to compute based on the prime factorization of $$m$$ (see the Wikipedia article for details).

We can see directly from this definition that if $$b$$ is coprime to $$m$$, then $$q = 0$$ and $$r = \lambda(m)$$ satisfy the rule since

$$b^0 \equiv b^{\lambda(m)} \equiv 1 \pmod{m}$$

But what about if $$b$$ is not coprime to $$m$$? It turns out that there is a more general property. If $$k$$ is the maximum exponent in the prime factorization of $$m$$, then:

$$ b^k \equiv b^{k + \lambda(m)} \pmod{m} $$

In other words, we can choose $$q = k$$ and $$r = k + \lambda(m)$$.

Note: These $$q, r$$ are not necessarily minimal since they are independent of $$b$$. For example, if we are evaluating $$13^n \;\%\; 4$$ then since $$13 \equiv 1 \pmod{4}$$, we could have chosen $$q = 0, r = 1$$, whereas using the property above will lead to $$q = 2, r = 5$$. However, in practice this method seems to be efficient enough.


## Example Computation

Putting this all together, let's consider Pavel's [BB(2, 6) > 10↑↑91](https://groups.google.com/g/busy-beaver-discuss/c/QBC_0X5Re3I/m/0E7EazrMAgAJ) TM. In order to simulate this TM you must at some point compute

$$ r = \frac{61 \cdot 2^{n_1} - 86}{3} \;\%\; 3 $$

where $$n_1$$ is an 89-layer recursive formula of the form:

$$ n_k = \frac{61 \cdot 2^{n_{k+1}} - a_k}{9} $$

$$ n_{89} = 105 $$

for some integers $$a_k$$ which need not be listed here.

$$ r = \frac{61 \cdot 2^{n_1} - 86}{3} \;\%\; 3 = \frac{(61 \cdot 2^{n_1} - 86) \;\%\; 3^2}{3} $$

so we need to compute $$ 2^{n_1} \;\%\; 3^2 $$. $$\lambda(3^2) = 2 \cdot 3$$ and using the Carmichael method described above we get $$q = 2, r = 2 + 2 \cdot 3, d = 2 \cdot 3$$ so the next step is to compute $$k_1 = (n_1 - q)\%d + q$$

$$ (n_1 - 2)\%(2 \cdot 3) = \frac{61 \cdot 2^{n_2} - (a_1 + 18)}{9} \;\%\; (2 \cdot 3) =  \frac{(61 \cdot 2^{n_2} - (a_1 + 18)) \;\%\; (2 \cdot 3^3)}{9} $$

which requires us to compute $$ 2^{n_2} \;\%\; (2 \cdot 3^3) $$. This process continues, with us recursively attempting to compute $$ 2^{n_k} \;\%\; (2 \cdot 3^{k+1}) $$ until we finally get to $$ 2^{n_{89}} \;\%\; (2 \cdot 3^{90}) = 2^{105} $$. Which allows us to work our way back down the tower which looks like this:




## Footnotes
