---
layout: post-custom
title: What is Collatz-hard?
tags: collatz
---

My recent [BB(3, 3) is Hard]({% post_url 2023-10-16-bb-3-3-is-hard %}) got quite a bit of attention. It was shared on [Hacker News](https://news.ycombinator.com/item?id=37910297) and [Reddit r/math](https://www.reddit.com/r/math/comments/17ai73o/bb3_3_is_hard/) and there continues significant ongoing discussion about Bigfoot on the [bbchallenge.org Discord server](https://discord.gg/3uqtPJA9Uv).  My article was the #1 post on Hacker News for several hours early Tuesday morning, received over 100 comments and I even got an email from a friend (who I haven't talked to in over a decade) after they saw it there. It is really gratifying and rewarding for me to see so much interest around one of my posts! It was especially touching to see hear strangers on the Internet saying nice things, like

> [throwawa14223](https://news.ycombinator.com/item?id=37910577): I appreciate the authors writing style. It helped me understand the subject without seeming verbose. That sweet spot can be difficult.

or even

> [haltist](https://news.ycombinator.com/item?id=37910498): Pretty good writeup.

There were also several questions about how we should think about Collatz-like problems and and what it means to reduce a TM to them. I wrote short responses to these comments on the forum, but felt that it would be valuable to bring them all together here and write a bit more long-form on my point of view.


## Expertise

I'd like to start by saying that I am **not** a Collatz expert! I am a recreational mathematician with an undergraduate math degree. I've been studying Busy Beavers as a hobby for the last 18 years and I feel relatively confident in saying that I'm a Busy Beaver expert at this point. It was through reading Pascal Michel's extensive analysis of the [Collatz-like Behavior of Busy Beavers](https://bbchallenge.org/~pascal.michel/beh) that I first became acquainted and interested with Collatz-like problems.


## Is Collatz-hard the new NP-hard

> [Master_Sergeant](https://www.reddit.com/r/math/comments/17ai73o/comment/k5f71t8/): Is "reduces to a Collatz-type problem" the new "cannot be solved in polynomial time unless P = NP"

Yes, well, aspirationally yes.

In computability theory, we have this well-established concept of a problem being [NP-hard](https://en.wikipedia.org/wiki/NP-hardness) which means that this problem cannot be solved with a polynomial time (i.e. efficient) algorithm unless [P = NP](https://en.wikipedia.org/wiki/P_versus_NP_problem). "P vs. NP" question is a famously difficult open problem. We generally believe that P ≠ NP and so believe that demonstrating that a problem is NP-hard is a meaningful statement. However, if it turns out that P = NP, then every polynomial time problem is NP-hard and so demonstrating that a problem is NP-hard is not particularly meaningful. So, NP-hard is a well-defined property that is meaningful to prove assuming a well-studied open problem in math is true.

In the context of Busy Beaver analyses, I am thinking about a similar sort of idea of a TM being "Collatz-hard". Loosely the idea here is that this TM is as hard to prove as a "Collatz-like problem". So, is this a "well-defined property that is meaningful to prove assuming a well-studied open problem in math is true"? Well, not exactly. It depends on a precise definition of what a "Collatz-like problem" is and whether every problem in that class is a "well-studied open problem in math".

I think that it is safe to say that if we can reduce a TM to **the** Collatz conjecture, then that is a well defined property that is meaningful to prove assuming a well-studied open problem in math is true. However, it turns out to be rather difficult to find a TM (starting on a blank tape) will only halt if the Collatz conjecture is true. In practice we often find the TM reducing to a problem similar to the Collatz conjecture (a Collatz-like problem).


## Is this Collatz-like problem really hard?

> [nneonneo](https://news.ycombinator.com/item?id=37910942): Probably more accurate to say that BB(3, 3) looks hard; that is, it encodes a Collatz-type problem, and many Collatz-type problems are very hard to solve (including, of course, the classic Collatz conjecture).
> However, this instance might not necessarily be hard. For one, the behaviour seems to be heavily biased; for another, we only have to consider a single trajectory instead of the trajectories for all integers (as with the classic Collatz problem).


### Background

The classic Collatz conjecture asks the question:

  * Given the function:

$$ \begin{array}{l}
  D & : & 2k   & \mapsto &  k \\
    &   & 2k+1 & \mapsto & 3k+2 \\
\end{array} $$

  * For all $$n \in \mathbb{Z}^+$$, does there exists an $$r$$ such that $$D^r(n) = 1$$? In other words, do all trajectories converge?

The Collatz-like problem I described is:

  * Given the function:

$$ \begin{array}{l}
  A & : & (a, & 6k,   & c) & \mapsto & (a,   & 8k+c-1, & 2)   \\
    &   & (a, & 6k+1, & c) & \mapsto & (a+1, & 8k+c-1, & 3)   \\
    &   & (a, & 6k+2, & c) & \mapsto & (a-1, & 8k+c+3, & 2)   \\
    &   & (a, & 6k+3, & c) & \mapsto & (a,   & 8k+c+1, & 5)   \\
    &   & (a, & 6k+4, & c) & \mapsto & (a+1, & 8k+c+3, & 2)   \\
    &   & (a, & 6k+5, & c) & \mapsto & (a,   & 8k+c+5, & 3)   \\
\end{array}$$

  * Do there exist $$r, b, c$$ such that $$A^r(2, 1, 2) = (-1, b, c)$$? In other words, does a single trajectory converge?


### Trajectories

In the classic `3n + 1` Collatz conjecture, proving a single trajectory converges is generally not very hard. Assuming the Collatz conjecture is true, it simply involves repeatedly applying the function until you reach 1. In fact, it is relatively efficient, the number of applications needed grows quite slowly. For example, for all numbers $$n \le 10^{19}$$, the "delay" ($$r$$ such that $$D^r(n) = 1$$) is less than 2500 (according to [Eric Roosendaal's list of delay records](http://www.ericr.nl/wondrous/delrecs.html)).

However, consider the `5n + 1` problem:

$$
F(n) = \begin{cases}
  \frac{n}{2}    & \text{if } n \equiv 0 \pmod{2} \\
  \frac{5n+1}{2} & \text{if } n \equiv 1 \pmod{2} \\
\end{cases}
$$

This is (arguably) the the second most well studied Collatz-like problem. Here it is believed that most trajectories diverge (eventually grow without bound). But (as far as I know) nobody has proven that even one specific trajectory diverges. Proving a trajectory diverges is much harder than showing it converges. Convergence can be demonstrated simply by finite applications of the map. Divergence requires proof of general structure of the numbers in the trajectory.

In this sense, proving that a single trajectory diverges is somewhat akin to proving that all trajectories converge. They both depend upon proving some general structure that an infinite number of cases fit into.


### Iterated Function

TODO: Which functions are really hard?



#### Biased Function



TODO: Is A a biased function? C is not and is equivalent.



## Collatz-like problems are Turing Complete

> [FartyMcFarter](https://news.ycombinator.com/item?id=37913095): I thought John Conway already proved that all instances of the halting problem can be converted to a Collatz-like problem [1]. So one could say this about all BB values, not just BB(3, 3). Some will be easy, some will be hard, but all are reducible to Collatz-like problems IIUC.
> [1] https://julienmalka.me/collatz.pdf
