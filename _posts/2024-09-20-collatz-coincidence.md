---
layout: post-custom
title: One Collatz Coincidence
tags: busy-beaver champion collatz
---

A common question people have about Busy Beaver champions is: "What are they doing?" This question sounds sort of objective, but is really a subjective philosophical question. Objectively, the TM is repeatedly evaluating the specific transition table rules until it halts, that's all. In a sense, this is the "simplest" (smallest) explanation because these champions are literally the ones that "do the most" based on the smallest description.

But this is not a very satisfying answer. The real meaning of this question is not to get the "smallest" explanation, but one that is more coherent or insightful to us as humans. However, must there be a more insightful description of behavior of these champion machines? Before getting into Busy Beaver analysis, I would have expected that they would not generally have such an insightful description. And I was not alone here: In his 2020 survey ["The Busy Beaver Frontier"](https://scottaaronson.blog/?p=4916) Scott Aaronson conjectures:

> A related intuition, though harder to formalize, is that Busy Beavers shouldn’t be “cleanly factorizable” into main routines and subroutines—but rather, that the way to maximize runtime should be via “spaghetti code,” or a single n-state amorphous mass.

And yet, as many readers of this blog will be well aware, we can actually describe the behavior of most known Busy Beaver champions in a way that seems relatively simple mathematically. In fact, [Pascal Michel discovered](https://doi.org/10.1007/BF01409968) in 1993 that many of the top BB(5) TMs simulate iterations of relatively simple Collatz-like functions and he has extended this analysis to many other champions with different numbers of states and symbols on his [Behavior of busy beavers](https://bbchallenge.org/~pascal.michel/beh) website. This result led Nick Drozd to claim that [Maybe the Spaghetti Code Conjecture is False](https://nickdrozd.github.io/2021/09/25/spaghetti-code-conjecture-false.html) in 2021.

But is this just the [streetlight effect](https://en.wikipedia.org/wiki/Streetlight_effect)? Presumably we are biased towards discovering Turing machines and proving they halt when they have simpler abstractions.[^bias] With the recent solution of [BB(5)](https://discuss.bbchallenge.org/t/july-2nd-2024-we-have-proved-bb-5-47-176-870/237) and [BB(2,4)]({% post_url 2024-08-27-bb-2-4-proven %}) problems, we now know with certainty that the S(5) and S(2,4) champions are both described by Collatz-like trajectories.

[^bias]: In fact, I expect that this is the case with BB(6) where the current champion runs for so long that we cannot even write the number using standard scientific notation and need to use Knuth up-arrows. The only reason we could find this champion, was because we were able to find patterns which allowed accelerating the simulation enough to prove it halts.

... or perhaps I should say Collatz-like **trajectory**. Because fellow Discord user Racheline discovered on 4 Sep 2024 ([Discord](https://discord.com/channels/960643023006490684/1260977480316092527/1280966930395828348)) that they are both effectively simulating the same Collatz-like trajectory! In fact, this Collatz-like trajectory appears to be ubiquitous among Turing machines around this size.


## Collatz Analyses
### S(5) Champion

The S(5) champion `1RB1LC_1RC1RB_1RD0LE_1LA1LD_1RZ0LA` runs for 47,176,870 steps and leaves 4098 marks on the tape.[^bb5_marks] It was discovered by Heiner Marxen and Jürgen Buntrock in 1989. Pascal Michel [provided the following analysis](https://bbchallenge.org/~pascal.michel/beh#tm52a) in 1993:

[^bb5_marks]: Note: `1RB1RA_1LC1LB_1RA1LD_1RA1LE_1RZ0LC` also leaves 4098 marks so technically there are actually two Σ(5) champions.

Let

$$ M(n) = 0^\infty \; \text{<A} \; 1^n \; 0^\infty $$

then

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & = & M(0) \\
  M(3k)   & \to & M(5k+6) \\
  M(3k+1) & \to & M(5k+9) \\
  M(3k+2) & \to & 0^\infty \;\; 1 \;\; \text{Z>} \;\; 01 \;\; {(001)}^{k+1} \;\; 1 \;\; 0^\infty  \\
\end{array}$$

The full trajectory from M(0) is:

$$\begin{array}{l}
  M(0)    & \to & M(6)    & \to & M(16)   & \to & M(34)   & \to & M(64)    & \to & \\
  M(114)  & \to & M(196)  & \to & M(334)  & \to & M(564)  & \to & M(946)   & \to & \\
  M(1584) & \to & M(2646) & \to & M(4416) & \to & M(7366) & \to & M(12284) & \to & \text{HALT}
\end{array}$$


### S(5) Runner Up

The second longest running BB(5) TM `1RB0LD_1LC1RD_1LA1LC_1RZ1RE_1RA0RB` runs for 23,554,764 steps and leaves 4097 marks on the tape. It was also discovered by Marxen and Buntrock in 1989. Pascal Michel also [provided the following analysis](https://bbchallenge.org/~pascal.michel/beh#tm52b) in 1993:

Let

$$ B(n) = 0^\infty \; \text{<A} \; 1^n \; 0^\infty $$

then

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & = & B(0) \\
  B(3k)   & \to & B(5k+3) \\
  B(3k+1) & \to & 0^\infty \;\; 1 \;\; {(110)}^k \;\; 11 \;\; \text{Z>} \;\; 0^\infty  \\
  B(3k+2) & \to & B(5k+7) \\
\end{array}$$

The full trajectory from B(0) is:

$$\begin{array}{l}
  B(0)    & \to & B(3)    & \to & B(8)    & \to & B(17)   & \to & B(32)    & \to & \\
  B(57)   & \to & B(98)   & \to & B(167)  & \to & B(282)  & \to & B(473)   & \to & \\
  B(792)  & \to & B(1323) & \to & B(2208) & \to & B(3683) & \to & B(6142)  & \to & \text{HALT}
\end{array}$$

If you compare this with the M trajectory, a pattern immediately jumps out. You can directly convert between these trajectories by replacing $$B(n)$$ with $$M(2n)$$! And, in fact, we can prove this:

$$\begin{array}{lcl}
  M(2(3k))     & = & M(3(2k))       & \to & M(5(2k) + 6) & = & M(2(5k + 3)) \\
  M(2(3k + 1)) & = & M(3(2k) + 2)   & \to & \text{Halt} \\
  M(2(3k + 2)) & = & M(3(2k+1) + 1) & \to & M(5(2k+1) + 9) & = & M(2(5k + 7)) \\
\end{array}$$

Therefore:

$$\begin{array}{lcl}
  M(2a) \to M(2b)       & \iff & B(a) \to B(b) \\
  M(2a) \to \text{Halt} & \iff & B(a) \to \text{Halt} \\
\end{array}$$

But wait, there's more! In fact, there is actually a giant clump of [19 distinct BB(5) TMs](https://wiki.bbchallenge.org/wiki/BB(5)#Enumeration) that run > 10 million steps and all leave between 4096-4098 marks on the tape!  Pascal analyzed the other Σ(5) champion (`1RB1RA_1LC1LB_1RA1LD_1RA1LE_1RZ0LC`) in his 1993 paper and showed that it simulates the exact same function. I haven't analyzed the rest of them, but I'd gues that they are all taking advantage of equivalent Collatz-like trajectories as well (since they are clumped so tightly).


### BBB(4) Champion

The current 4-state [Beeping Busy Beaver](https://wiki.bbchallenge.org/wiki/Beeping_Busy_Beaver) (and also [Blanking Beaver](https://nickdrozd.github.io/2021/02/14/blanking-beavers.html)) champion `1RB1LD_1RC1RB_1LC1LA_0RC0RD` runs 32,779,478 steps before becoming a trivial [Translated Cycler](https://wiki.bbchallenge.org/wiki/Translated_Cycler) with period 1. It was [discovered](https://nickdrozd.github.io/2021/07/11/self-cleaning-turing-machine.html) by Nick Drozd in 2021. I first [analyzed]({% post_url 2021-07-17-bb-collatz %}) it way back as my second Busy Beaver related blog post here. I actually forgot about that analysis (oops) and [re-analyzed](https://wiki.bbchallenge.org/wiki/1RB1LD_1RC1RB_1LC1LA_0RC0RD) it last week! Racheline discovered the connection with the S(5) champion's iteration.

Let

$$ D(n) = 0^\infty \; 1^n \; 0^4 \; \text{D>} \; 0^\infty $$

then

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & \xrightarrow{19} & D(2) \\
  D(3k)   & \to & 0^\infty \; \text{<C} \; 0^\infty  & \text{(Quasihalt)} \\
  D(3k+1) & \to & D(5k+6) \\
  D(3k+2) & \to & D(5k+7) \\
\end{array}$$

the full trajectory from D(2) is:

$$\begin{array}{l}
          &     & D(2)    & \to & D(7)    & \to & D(16)   & \to & D(31)    & \to & \\
  D(56)   & \to & D(97)   & \to & D(166)  & \to & D(281)  & \to & D(472)   & \to & \\
  D(791)  & \to & D(1322) & \to & D(2207) & \to & D(3682) & \to & D(6141)  & \to & \text{HALT}
\end{array}$$

and finally, compare this to the $$B(n)$$ trajectory and the pattern becomes clear, just replace each $$D(n)$$ with $$B(n+1)$$! And once again, it's not too hard to prove this (once you see the pattern):

$$\begin{array}{lcl}
  B(3k     & + 1) & \to & \text{Halt} \\
  B(3k + 1 & + 1) & \to & B(5k+6 & + 1) \\
  B(3k + 2 & + 1) & \to & B(5k+7 & + 1) \\
\end{array}$$

so

$$\begin{array}{lcl}
  D(a) \to D(b)        & \iff & B(a+1) \to B(b+1) \\
  D(a) \to \text{Quasihalt} & \iff & B(a+1) \to \text{Halt} \\
\end{array}$$


### S(2,4) Champion

The S(2,4) champion `1RB2LA1RA1RA_1LB1LA3RB1RZ` runs for 3,932,964 steps and leaves 2050 marks on the tape. It was discovered by my dad, Terry, and I in 2005. Racheline discovered the connection with the S(5) champion's iteration. The analysis here will require a little more work to connect the dots.

Let

$$ L(a, b, c) = 0^\infty \; 1 \; 3^a \; \text{<A} \; 2^b \; 1^c \; 0^\infty $$

then

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & \xrightarrow{5} & L(0, 0, 2) \\
  L(a+1, b, c+1) & \to & L(a, b+2, c) \\
  L(a+1, b, 0)   & \to & L(a, b+1, 2) \\
  L(0,   b, c+1) & \to & L(b+1, 0, c+1) \\
  L(0,   b, 0)   & \to & \text{Halt} \\
\end{array}$$

At this point it might not be obvious that there is any connection. Racheline discovered that the trick is to transform the configuration a bit. Let

$$ R(a, 3k+r) = L(a, k, r) $$

then, it turns out that

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & \xrightarrow{5} & R(0, 2) \\
  R(a+1, b) & \to & R(a, b+5) \\
  R(0, 3k) & \to & \text{Halt} \\
  R(0, 3k+r) & \to & R(k+1, r) & \text{if } r > 0 \\
\end{array}$$

This second rule is where the magic is! Note that the $$R(a+1, b)$$ rule effectively combines the two $$L(a+1, b, c)$$ cases above. Now, we can simplify this to:

$$\begin{array}{lcl}
  R(0, 3k) & \to & \text{Halt} \\
  R(0, 3k+r) & \to & R(0, 5k+5+r) & \text{if } r > 0 \\
\end{array}$$

let $$ R(n) = R(0, n) $$ then

$$\begin{array}{lcl}
  0^\infty \; \text{<A} \; 0^\infty & \xrightarrow{5} & R(2) \\
  R(3k)   & \to & \text{Halt} \\
  R(3k+1) & \to & R(5k+6) \\
  R(3k+2) & \to & R(5k+7) \\
\end{array}$$

and we can see that these $$R(n)$$ rules are identical to the $$D(n)$$ rules above (and also starts at the same point), so it will also follow an equivalent trajectory.


# The Collatz Coincidence

Why are all of these champion TMs following equivalent Collatz-like trajectories? Well, this specific Collatz-like trajectory is unusually "lucky". Let's focus on the $$B(n)$$ trajectory:

$$\begin{array}{l}
  B(0)    & \to & B(3)    & \to & B(8)    & \to & B(17)   & \to & B(32)    & \to & \\
  B(57)   & \to & B(98)   & \to & B(167)  & \to & B(282)  & \to & B(473)   & \to & \\
  B(792)  & \to & B(1323) & \to & B(2208) & \to & B(3683) & \to & B(6142)  & \to & \text{HALT}
\end{array}$$

This avoids the halt transition 14 times before finally halting. If we treat each transition as having a uniform random chance of hitting each of the 3 rules, then the chance of avoiding halt 14 times in a row is $$(\frac{2}{3})^{14} \approx 0.0034 \approx \frac{1}{300}$$. So, it seems like this is pretty rare. Perhaps rare enough that any TM which can (just barely) simulate it outcompetes all others of it's size?


## Footnotes