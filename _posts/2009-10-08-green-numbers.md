---
layout: post
title: Green Numbers
tags: busy-beaver big-number-notation
---

(This article was [original published](https://en.wikipedia.org/wiki/User:Sligocki/Green%27s_numbers) on in Oct 2009 on my Wikipedia User page. I'm re-posting on my blog to bring my writing together in one place.)

Milton Green discovered a class of [Turing machines](https://en.wikipedia.org/wiki/Turing_machine) which produce extremely large results in the [busy beaver](https://en.wikipedia.org/wiki/Busy_beaver) game.[^green64]

The machines defined recursively and the number of 1s they leave on the tape is likewise defined recursively. We examine those numbers here:


## Definition
Let us define the numbers $$B_n$$ for $$n$$ odd.
* $$ B_n(0) = 1 $$
* $$ B_1(m) = m+1 $$
* $$ B_n(m) = B_{n-2}[B_n(m-1) + 1] + 1 $$

Then, Green's numbers $$BB_n$$ are defined as:
* $$ BB_n = B_{n-2}[B_{n-2}(1)] $$ for odd $$n$$
* $$ BB_n = B_{n-3}[B_{n-3}(3) + 1] + 1 $$ for even $$n$$


## Examples

* $$ B_1(m) = m+1 $$
* $$ B_3(m) = 3m + 1 $$
* $$ B_5(m) = \frac{7}{2} \cdot 3^m - \frac{5}{2} $$
* $$ B_7(m) > 3 \uparrow\uparrow m $$  and  $$ B_7(1) = B_5(2) + 1 = 29 $$
* $$ B_9(m) > 3 \uparrow\uparrow\uparrow m $$  and  $$ B_9(1) = B_7(2) + 1 = B_5(30) + 2 = 720618962331271 $$

* $$ BB_3 = 3 $$
* $$ BB_4 = 7 $$
* $$ BB_5 = 13 $$
* $$ BB_6 = 35 $$
* $$ BB_7 = B_5(8) = 22961 $$
* $$ BB_8 = B_5(93) + 1 = 3(7 \cdot 3^{92} - 1) / 2 = 824792557184288824246737061810550733633916929 $$
* $$ BB_9 = B_7(B_7(1)) = B_7(29) > 3 \uparrow\uparrow 29 > 10 \uparrow\uparrow 28 $$
* $$ BB_{10} = B_7(B_7(3) + 1) + 1 > 3 \uparrow\uparrow 3 \uparrow \uparrow 3 = 3 \uparrow\uparrow\uparrow 3 $$
* $$ BB_{11} = B_9(B_9(1)) = B_9(720618962331271) > 3 \uparrow\uparrow\uparrow 720618962331271 $$


## Bounds
We will show that $$B_n(m)$$ grows like $$ 3 \uparrow^{n/2} m $$ and furthermore that $$BB_n$$ grows like $$ 3 \uparrow^{n/2} 3 $$ (See [Knuth's up-arrow notation](https://en.wikipedia.org/wiki/Knuth%27s_up-arrow_notation) and [Up-arrow Properties]({% post_url 2009-10-07-up-arrow-properties %})).


Claim: $$ B_{2k+3}(m) \ge 3 \uparrow^k m $$ for any $$k \ge -2$$ and $$m \ge 0$$

Proof by induction:

Base Case:

$$ B_{2k+3}(0) = 1 \ge 1 = 3 \uparrow^k 0 $$

$$ B_1(m) = m+1 \ge m+1 = 3 \uparrow^{-2} m $$

Inductive Step:

Assume that $$ B_{2k^\prime+3}(m^\prime) \ge 3 \uparrow^{k^\prime} m^\prime $$ (for all $$ k^\prime = k, m^\prime < m $$ or $$ k^\prime < k, m^\prime < 3 \uparrow^k (m-1) $$)

$$ B_{2k+3}(m) = B_{2k+1}[B_{2k+3}(m-1) + 1] + 1 > B_{2k+1}[3 \uparrow^k (m-1)] > 3 \uparrow^{k-1} [3 \uparrow^k (m-1)] = 3 \uparrow^k m \,$$

**QED**


Claim: $$ B_{2k+3}(m) < \frac{1}{2} (3 \uparrow^k (m+2)) < (3 \uparrow^k (m+2)) $$ for any $$k \ge 1$$ and $$m \ge 0$$ (In fact the bound can be tightened to $$m$$+1 for $$k$$ ≥ 2)

Proof by induction:

Base Case:

$$ B_{2k+3}(0) = 1 < \frac{1}{2} (3 \uparrow^k 2) $$

$$ B_5(m) = \frac{9}{2} 3^m < \frac{1}{2} (3 \uparrow (m+2)) $$

$$ B_7(m) < \frac{1}{2} (3 \uparrow^2 (m+1)) $$

(left as an exercise to the reader)

Inductive Step:

Assume that $$ B_{2k^\prime+3}(m^\prime) < \frac{1}{2} (3 \uparrow^{k^\prime} (m^\prime+2)) $$ (for $$ k^\prime = k, m^\prime < m $$ or $$ k^\prime < k, m^\prime < 3 \uparrow^k m + 1 $$)

$$ \begin{array}{rll}
  B_{2k+3}(m) = B_{2k+1}[B_{2k+3}(m-1) + 1] + 1 & < & B_{2k+1}[\frac{1}{2} (3 \uparrow^k (m+1)) + 1] + 1   \\
                                                & < & \frac{1}{2} (3 \uparrow^{k-1} [\frac{1}{2} (3 \uparrow^k (m+1)) + 3]) + 1  \\
                                                & \le? & \frac{1}{2} (3 \uparrow^{k-1} [\frac{1}{2} (3 \uparrow^k (m+1)) + 4])  \\
                                                & \le? & \frac{1}{2} (3 \uparrow^{k-1} (3 \uparrow^k (m+1)))  \\
                                                & = & \frac{1}{2} (3 \uparrow^k (m+2))
\end{array} $$

≤? statements seem obvious, but grungy to prove (left as an exercise to the reader :) )


Claim: $$ 3 \uparrow^k 3 < BB_{2k+4}, BB_{2k+5} < 3 \uparrow^{k+1} 3 $$ for $$k \ge 1$$

Proof:

$$ BB_{2k+5} = B_{2k+3}(B_{2k+3}(1)) > 3 \uparrow^k (3 \uparrow^k 1) = 3 \uparrow^k 3 $$

$$ BB_{2k+4} > B_{2k+1}(B_{2k+1}(3)) > 3 \uparrow^{k-1} (3 \uparrow^{k-1} 3) = 3 \uparrow^k 3 $$

$$ BB_{2k+5} = B_{2k+3}(B_{2k+3}(1)) < \frac{1}{2} (3 \uparrow^k (\frac{1}{2} (3 \uparrow^k (1+2))+2)) < 3 \uparrow^k (3 \uparrow^k 3) = 3 \uparrow^{k+1} 3 $$

$$ BB_{2k+4} = B_{2k+1}[B_{2k+1}(3) + 1] + 1 < \frac{1}{2} (3 \uparrow^{k-1} (\frac{1}{2} (3 \uparrow^{k-1} (3+2))+3)) + 1 < 3 \uparrow^{k-1} (3 \uparrow^{k-1} 5) < 3 \uparrow^k 4 < 3 \uparrow^{k+1} 3 $$

**QED**

## References

[^green64]: Milton W. Green. "A Lower Bound on Rado's Sigma Function for Binary Turing Machines", _Preceedings of the IEEE Fifth Annual Symposium on Switching Circuits Theory and Logical Design_, 1964, pages 91–94, doi: [10.1109/SWCT.1964.3](https://doi.org/10.1109%2FSWCT.1964.3).
