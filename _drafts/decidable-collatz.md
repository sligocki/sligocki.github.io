---
layout: post-custom
title: A Decidable Collatz-like Problem
tags: collatz
---

I have shared many Collatz-like problems on this blog and generally with the implication that simply because they were Collatz-like, they were hard. I believe that this judgement is generally justified, but it is hard to figure out how to even specify something like that in a more objective/definitive way. I am no expert in the Collatz conjecture or Number theory in general, I'm simply a well-read amateur with a special interest. But over my years off-and-on of reading about Collatz related results I have sort of come to adopt the intuitive heuristic that there are roughly 2 categories of questions you can ask: (A) Ones in which the answer is extremely easy to solve and (B) Ones that nobody has any idea on how to solve.

Some examples of the first type are:

1. If you take the odd branch of the non-reduced Collatz function $C(n) = 3n+1$ you will always end up with an even number (since $C(2k+1) = 6k+4$ is clearly even).
2. The iterations of the [Bigfoot]({% post_url 2023-10-16-bb-3-3-is-hard %}) rule cycles through the pattern "odd, odd, even, even" for the $b$ parameter.
3. [Almost all numbers go south](https://www.youtube.com/watch?v=ifizml-ZUqY): Almost all (measure 1) positive integers ($n$) eventually go to a smaller number in the classic Collatz problem ($\exists k, \; C^k(n) < n$).

Some examples of the second type are: The Collatz conjecture, the fate of 7 using the "5n+1" rule,[^five-n] the fate of Hydra and Antihydra.

[^five-n]: The 5n+1 rule is like the 3n+1 rule, but with $C_5(n) = 5n+1$ for odd n. For even n you still have $C_5(n) = n/2$.

Of course, I am being a bit hyperbolic here. Obviously there are many non-trivial proven results related to the Collatz conjecture. Some examples include:

1. The proof that [no (non-trivial) circuits exist](https://www.youtube.com/watch?v=MZv5L8iaqlQ) (3n+1 cycles which follow the pattern odd, odd, ..., odd, even, even, ... even) which depends upon Baker's Theorem and the bounds it implies on the [separations between powers of 2 and powers of 3](https://terrytao.wordpress.com/2011/08/21/hilberts-seventh-problem-and-powers-of-2-and-3/).
2. [Almost all Collatz orbits attain almost bounded values](https://terrytao.wordpress.com/2019/09/10/almost-all-collatz-orbits-attain-almost-bounded-values/).

But these results are rather technical in character ...


Then I heard about the Farkas Collatz-like function