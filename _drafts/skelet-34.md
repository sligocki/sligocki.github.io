---
layout: post
title: >
  Skelet #34 is Infinite
tags: busy-beaver
---

In 2003, Georgi Georgiev (mostly known by his pseudonym "Skelet") [shared](http://skelet.ludost.net/bb/index.html) his very impressive results in his search to prove $$BB(5)$$. Specifically, he claimed to have proven all TMs except for [43 holdouts](https://skelet.ludost.net/bb/nreg.html) (which he called "Hardly Non Regular" HNR). In the Busy Beaver community, his list of 43 machines has been regarded with a mixture of amazement, suspicion and hope over the last 20 years. Amazement because no other person has ever reported anywhere close to that result (for reference, as of this Summer, my $$BB(5)$$ search had over 7k holdouts). Suspicion because his evidence was only a single 6000 line uncommented Pascal program. And hope because it seemed like this meant that proving $$BB(5)$$ could be "within reach". [bbchallenge.org](https://bbchallenge.org/skelet) maintains a canonical ordering of these HNR TMs and so they are often referred to simply as "Skelet #N".

Whether or not you trust Skelet's program completely: these 43 are certainly examples of some of the most difficult $$BB(5)$$ TMs to analyze and prove. And as the [bbchallenge.org](https://bbchallenge.org/story) community has pushed towards it's own attempt to prove $$BB(5)$$, many of these Skelet HNR TMs continue to appear in our lists of holdouts. Many people have devoted significant time to analyzing and proving these machines by hand. Of particular note, Dan Briggs has a [project](https://github.com/danbriggs/Turing) to write proofs for these 43 Skelet holdouts and [claims](https://github.com/danbriggs/Turing/blob/master/paper/HNRs.pdf) to have reduced the list to 21.

Last week I dove deeply into analyzing one of these remaining 21 TMs (Skelet \#34) by hand and I present the proof that it is infinite here. As far as I know, this is the first proof of this machine.


## The Machine

The TM I will be discussing here is Skelet \#34 which he describes with the notation `C1L D1R  E1R H1L  D0L C0L  B1R A0R  A1R A1L`. Converted into Tree Normal Form (TNF) and using [Standard Text format]({% post_url 2022-10-09-standard-tm-format %}) this is:

`1RB1LC_0RC0RB_1LD0LA_1LE---_1LA1RA`

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LC |
|  B  | 0RC | 0RB |
|  C  | 1LD | 0LA |
|  D  | 1LE | --- |
|  E  | 1LA | 1RA |

For the rest of this article, I will use this TNF state naming.


## Behavior Overview

Broadly, this TM alternates between 2 "phases". It spends the majority of it's time in the "Counter Phase": simulating a two-sided binary counter. It stays in the Counter Phase until the right side counter "expands" (reaches a new power of 2) at which point it transitions to the "Reset Phase". In the Reset Phase it follows a more complicated set of rules before starting the Counter Phase again. The proof that this TM never halts hinges on proving that the Reset Phase always finishes relatively quickly.

### Binary Expansion Countdown

It will be useful to define a function before we begin that expresses a sort of "countdown" until the next binary expansion:

$$b(n) = \min \{ d \in \mathbb{Z}^+ | n + d = 2^k \} = 2^{\lfloor \log_2 (n) \rfloor + 1} - n$$

In english, $$b(n)$$ is the number we have to add to $$n$$ to reach the "next" power of 2 (strictly greater than $$n$$) or the smallest number which when added to $$n$$ "expands" the binary representation to a larger length. For example:

$$ \begin{array}{rcr}
b( 13) &=& 3 \\
b( 31) &=& 1 \\
b( 32) &=& 32 \\
b(138) &=& 118 \\
\end{array} $$


## Counter Phase

### Notation

To start, we need to define a bit of notation:

Let `L(n)` represent a big-endian binary counter using blocks `0000` and `1000` (to represent digits `0` and `1` respectively). Precisely, let's recursively define `L(n)` for any non-negative integer `n` by:
 * `L(0)` is the empty string
 * `L(2k) = L(k) 0000`
 * `L(2k+1) = L(k) 1000`

Let `R(m)` represent a little-endian binary counter using blocks `10` and `11`. Precisely:
 * `R(0)` is the empty string
 * `R(2k) = 10 R(k)`
 * `R(2k+1) = 11 R(k)`

Finally, let <code>D(n, m) = 0<sup>∞</sup> L(n) <C 1010 R(m) 0<sup>∞</sup></code>.


### Counter Rules

#### Left Counter

Theorem 1: <code>0<sup>∞</sup> L(n) <C -> 0<sup>∞</sup> L(n+1) B></code> for all `n`

Proof by induction:
 * `n = 0`: <code>0<sup>∞</sup> L(0) <C = 0<sup>∞</sup> <C -> 0<sup>∞</sup> 1000 B> = 0<sup>∞</sup> L(1) B></code> in 7 steps by direct simulation.
 * `n = 2k`: <code>0<sup>∞</sup> L(2k) <C = 0<sup>∞</sup> L(k) 0000 <C -> 0<sup>∞</sup> L(k) 1000 B> = 0<sup>∞</sup> L(2k+1) B></code> in 7 steps by direct simulation.
 * `n = 2k+1`: <code>0<sup>∞</sup> L(2k+1) <C = 0<sup>∞</sup> L(k) 1000 <C -> 0<sup>∞</sup> L(k) <C 1111</code> in 4 steps by direct simulation. Then, by the inductive hypothesis, <code>0<sup>∞</sup> L(k) <C 1111 -> 0<sup>∞</sup> L(k+1) B> 1111</code>. Finally, <code>0<sup>∞</sup> L(k+1) B> 1111 -> 0<sup>∞</sup> L(k+1) 0000 B> = 0<sup>∞</sup> L(2k+2) B></code> in 4 steps by direct simulation.

QED


#### Right Counter

Let's first note that the right side counter is a bit different from the left side counter. In the left-side counter the block representing the binary digit 0 was `0000` which is the same as the block repeated infinitely at the end of `L(n)` (<code>0<sup>∞</sup></code>) In other words, the TM doesn't have any way to distinguish between the "leading zeros" and the "internal zeros" on the left side counter. However, on the right side counter, binary digit 0 is internally represented by `10`, but the "leading zeros" are `00`. Because of this the TM can (and in fact does) act differently when reaching `10` (an internal zero) vs. `00` (a leading zero).

A counter only reaches the leading zero if every digit in it's binary representation is 1, i.e. only if $$b(m) = 1$$ (or equivalently, only if $$m = 2^k - 1$$). At first we will only consider the other case, where $$b(m) > 1$$ and so there is at least one internal zero (`10`) in the binary representation of $$m$$. Before getting to the counter behavior we need a few lemmas.

Lemma 2: <code>0 C> 11<sup>n</sup> 10 -> <A 0 10<sup>n</sup> 11</code>

Proof by induction:
 * Base case: `n = 0`: <code>0 C> 10 -> <A 0 11</code> in 8 steps by direct simulation.
 * Inductive case: <code>0 C> 11<sup>n+1</sup> 10 = 0 C> 11 11<sup>n</sup> 10 -> 11 0 C> 11<sup>n</sup> 10</code> in 6 steps. Then, by the inductive hypothesis, <code>11 0 C> 11<sup>n</sup> 10 -> 11 <A 0 10<sup>n</sup> 11</code>. Finally, <code>11 <A 0 10<sup>n</sup> 11 -> <A 0 10 10<sup>n</sup> 11 = <A 0 10<sup>n+1</sup> 11</code> in 2 steps.

Lemma 3: `R(m)` contains the block `10` in it's representation if and only if $$m + 1$$ is not an exact power of 2 (i.e. $$b(m) > 1$$).

 * I leave the proof as an exercise to the reader. This is equivalent to saying that the binary representation of a number `m` contains a `0` (ignoring leading zeros) if and only if $$m + 1$$ is not an exact power of 2.

Theorem 4: If $$b(m) > 1$$, then <code>0 C> R(m) 0<sup>∞</sup> -> <A 0 R(m+1) 0<sup>∞</sup></code>

Proof:
 * Note: As discussed in the previous paragraph, the condition $$b(m) > 1$$ is equivalent to saying that there exists at least one internal zero (`10`) in `R(m)`.
 * In other words, we can rewrite <code>R(m) = 11<sup>a</sup> 10 R(b)</code> from which we can see that $$m = \sum_{k=0}^{a-1} 2^k + b 2^{a+1} = 2^a + b 2^{a+1} - 1$$.
 * By Lemma 2, <code>11<sup>a</sup> 10 R(b) -> 10<sup>a</sup> 11 R(b)</code>.
 * Finally, we can see how <code>10<sup>a</sup> 11 R(b)</code> $$= R(2^a + b 2^{a+1}) = R(m+1)$$.

QED


#### Full Counter Rule

Combining these rules leads to the two-sided counter behavior succinctly described by these rules:

Corollary 5: For all $$b(m) > 1$$, $$D(n, m) \to D(n+1, m+1)$$.

Corollary 6: For any $$m$$, let $$d = b(m) - 1$$, then $$D(n, m) \to D(n+d, m+d)$$ and $$b(m+d) = 1$$ (i.e. $$m+d = 2^k - 1$$ for some $$k \in \mathbb{N}$$).

 * These follow directly from Theorems 1 & 4 and the facts that `B> 1010 -> 111 0 C>` in 18 steps and `111 <A 0 -> <C 1010` in 3 steps.


## Reset Phase

But what happens once $$b(m) = 1$$? This is when the TM enters the "Reset Phase". I have not found any way to summarize the behavior in the Reset Phase quite so succinctly as I have for the Counter Phase. And the subtlety here is key to the proof that this TM never halts.


### Overflow Behavior

Let's start by looking at what happens right after reaching $$b(m) = 1$$ (in other words, when the right counter would "expand" or "overflow").

Lemma 7: <code>110 C> 11<sup>k</sup> 0 -> <C 10<sup>k+1</sup> 11</code>

 * I leave this proof as an exercise to the reader.

So, let's say that we ended the "Counter Phase" in:

 * <code>D(9, 15) = 0<sup>∞</sup> 1000 0000 0000 1000 <C 1010 11<sup>4</sup> 0<sup>∞</sup></code>

Simulating forward we get:

 * <code>0<sup>∞</sup> 1000 0000 1000 0000 B> 1010 11<sup>4</sup> 0<sup>∞</sup></code> by Theorem 1
 * <code>0<sup>∞</sup> 1000 0000 1000 0000 1 110 C> 11<sup>4</sup> 0<sup>∞</sup></code> by direct simulation.
 * <code>0<sup>∞</sup> 1000 0000 1000 0000 1 <C 10<sup>5</sup> 11 0<sup>∞</sup></code> by Lemma 7.
 * <code>0<sup>∞</sup> 1000 0000 1000 000 <C 1011 10<sup>4</sup> 11 0<sup>∞</sup></code> by direct simulation.
 * <code>0<sup>∞</sup> 0100 0000 0100 0000 <C 1011 10<sup>4</sup> 11 0<sup>∞</sup></code> by rewriting so that left side has block size 4 again.

This configuration looks similar to the general Counter config $$D(a, b)$$, but with 2 main differences:

 1. The blocks on the left are "shifted" by 1, so what used to be `1000` is not `0100`.
 2. The middle section used to be `1010` and is now `1011`.

However, notice that the right side is still in the standard $$R(m)$$ format. It turns out that the (2) change (middle `1010` -> `1011`) is a minor detail that (surprisingly) has almost no change on behavior, but the (1) change (left block `1000` -> `0100`) will lead to a very different behavior.


#### Potential to Halt

I have mentioned a few times already that the proof that this TM does not halt depends on a timing issue. I'm not yet ready to explain this issue completely, but I can give you a little hint at this point. As noted above, after overflowing the right counter we ended up shifting the left blocks so that all the `1000` blocks became `0100` blocks. If this TM were to somehow shift any of these blocks once more (so they ended up as `0010` blocks), then the TM would have the potential to halt. Specifically:

 * `10 <C -> Halt` in 2 steps by direct simulation.

So, as we will see, the crux of the proof that this TM never halts will depend up on showing that we do not overflow the right counter again until after we have completed the "Reset Phase" and removed all `0100` blocks from the left side of the tape.


### Rules

First, we have a totally new rule about how the TM interacts with `0100` blocks on the left:

Lemma 8: <code>0100 1000<sup>k</sup> <C 10 -> <C 1010 10<sup>2k</sup> 11</code>

Second, we have a generalization of a rule from the Counter Phase:

Lemma 9: If $$b(m) > 1$$ and $$b(n) > 1$$, then `L(n) <C 101a R(m) -> L(n+1) <C 101a R(m+1)` for any $$a \in \{0, 1\}$$

 * I leave this proof as an exercise to the reader. We have already proven the case of `a = 1` in the Counter Phase section. The proof for `a = 0` is very similar. A priori I would not have guessed that we would have the same behavior in both of these cases, but it turns out that we do.

Corollary 10: For all $$k \in \mathbb{N}$$, $$a \in \{0, 1\}$$, and $$m$$ such that $$b(m) > 2^k - 1$$, then <code>0000<sup>k</sup> <C 101a R(m) -> 1000<sup>k</sup> <C 101a R(t)</code> where $$t = m + 2^k - 1$$

 * This follows directly from Lemma 9 when you note that the condition $$b(m) > 2^k - 1$$ assures that you can apply Lemma 9 $$2^k - 1$$ times.

Finally, combining Lemma 8 and Corollary 10 together we get:

Corollary 11: For all $$k \in \mathbb{N}$$, $$a \in \{0, 1\}$$, and $$m$$ such that $$b(m) > 2^k - 1$$, then <code>0100 0000<sup>k</sup> <C 101a R(m) -> <C 1010 10<sup>2k</sup> 11 10 1a R(t)</code> where $$t = m + 2^k - 1$$

So, Corollary 11 explains exactly how the the Reset Phase will operate. Each application of Corollary 11 "consumes" one `0100` block as long as the condition $$b(m) > 2^k - 1$$ holds. Ensuring that this condition always holds will be the critical part of this proof.


### Reset Invariant

In order to prove that the $$b(m) > 2^k - 1$$ condition in Corollary 11 will always hold while the TM simulates the Reset Phase. I will introduce an invariant condition:

A TM configuration satisfies the **Reset Invariant** (RI) if:

 1. It is in a configuration <code>0<sup>∞</sup> X<sub>k</sub> <C 101a R(m) 0<sup>∞</sup></code> (where <code>X<sub>k</sub></code> is any sequence of $$k$$ blocks each of which are either `0000` or `0100`) and
 2. $$b(m) > 2^k$$

Now, we strengthen Corollary 11 to show that it preserves the Reset Invariant:

Theorem 12: For any configuration satisfying the Reset Invariant and the conditions for Corollary 11, after applying Corollary 11, the Reset Invariant still applies.

Proof:
 * Since RI and Corollary 11 preconditions all apply we are in a config: <code>0<sup>∞</sup> X<sub>r</sub> 0100 0000<sup>k</sup> <C 101a R(m) 0<sup>∞</sup></code> with $$b(m) > 2^{r+k+1}$$
 * Applying Corollary 11, we end up in configuration: <code>0<sup>∞</sup> X<sub>r</sub> <C 1010 10<sup>2k</sup> 11 10 1a R(t) 0<sup>∞</sup></code> where $$t = m + 2^k - 1$$
 * We can rewrite as <code>0<sup>∞</sup> X<sub>r</sub> <C 1010 R(s) 0<sup>∞</sup></code> where $$s = t \cdot 2^{2k+3} + x$$ and $$x \le 2^{2k} + 2^{2k+2}$$ (To be precise $$x \in \{2^{2k}, 2^{2k} + 2^{2k+2}\}$$ depending on whether `a` is `0` or `1` respectively).
 * In order to prove that RI applies to this result we must prove that $$b(s) > 2^r$$. This is not hard to prove, but the math is a bit grungy. I will list the outline of the proof:

 $$ b(m) > 2^{r+k+1} > 2^k - 1 $$

 $$ b(t) = b(m) - 2^k - 1 = 2^k (2^{r+1} - 1) + 1 > 2^{r+k} $$

 $$ b(t \cdot 2^{2k+3}) = b(t) \cdot 2^{2k+3} > 2^{(r+k) + (2k+3)} = 2^{r+3k+3} > x $$

 $$ b(s) = b(t \cdot 2^{2k+3} + x) = b(t \cdot 2^{2k+3}) - x > 2^{r+3k+3} - (2^{2k} + 2^{2k+2}) > 2^{r+3k+1} > 2^r $$


### Full Reset Rule
