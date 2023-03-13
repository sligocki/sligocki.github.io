---
layout: post
title: >
  Skelet #1 is infinite ... we think
tags: busy-beaver skelet
---

At the end of my [original Skelet \#1 post]({% post_url 2023-02-25-skelet-1-wip %}), I listed my ideas on ways to move forward. Third on the list was working on an accelerated simulator. Pavel Kropitz built a custom simulator for this TM using the Counter Notation (with Parity Change) I mentioned in that article. They also added Counter acceleration using the rule I listed in that article and I added @uni-cycle acceleration. Together, all of these custom optimizations, we were able to run the simulation long enough that it reached an infinite cycle! So we believe we have demonstrated that this TM is infinite (in fact, that it is a [Translated Cycler](https://discuss.bbchallenge.org/t/decider-translated-cyclers/34) / is Lin Recurrent).

But since this requires running our custom simulator over $$2^{26}$$ (over 60M) simulator steps and no other existing simulator is able to run nearly this long, I feel somewhat reluctant to claim certainty that we do not have a bug in our simulation! In order to confirm this TMs behavior it would be great if someone else could independently implement a simulator so that we could compare the results. Towards that end, I will provide all the theory that I think anyone would need to implement such an accelerated simulator.


## Tape representation

We run our simulator at a level of abstraction above the low level bits and TM states. That abstraction is an extension of the [Counter Notation]({% post_url 2023-02-25-skelet-1-wip %}#counter-encoding) I described in the original post. Extended Counter Notation is any string of the form:

$$ L (x|D|P|C_0|C_1|C_2|C_3|F_0|F_1|F_2|F_3|G_0|G_1|G_2)^* \; (<|>) \; (x|D|C|P)^* \; R $$

Such a string represents a specific TM configuration. To decode it to a basic TM config, convert each of the blocks into raw tape symbols using the following conversion (where, notably, the conversion is slightly different on the left side and right side of the tape):

| Name   | Counter Code | Run-length | Raw Tape (Left) | Raw Tape (Right) |
| :------------- | :---------- | :----- | ------------------: | :-------- |
| Filler         | $$x$$       | `22`   |           `011 011` | `110 110` |
| Dot            | $$D$$       | `21`   |            `011 01` | `110 10`  |
| Digit 0        | $$C_0$$     | `232`  |      `011 0111 011` |           |
| Digit 1        | $$C_1$$     | `201`  |          `011 0 01` |           |
| Digit 2        | $$C_2$$     | `42`   |         `01111 011` |           |
| Digit 3 (Clot) | $$C_3 = C$$ | `11`   |             `01 01` | `10 10`   |
| Right End      | $$R$$       | `2$`   |                     | `110` $$0^\infty$$ |
| Left End       | $$L$$       | `^`    |        $$0^\infty$$ |           |
| Parity Change  | $$P$$       | `2`    |               `011` | `110`     |
|                | $$F_0$$     | `432`  |    `01111 0111 011` |           |
|                | $$F_1$$     | `401`  |        `01111 0 01` |           |
|                | $$F_2$$     | `62`   |       `0111111 011` |           |
|                | $$F_3$$     | `31`   |    `011 011111 011` |           |
|                | $$G_0$$     | `2332` | `011 0111 0111 011` |           |
|                | $$G_1$$     | `2301` |     `011 0111 0 01` |           |
|                | $$G_2$$     | `252`  |    `011 011111 011` |           |

Also `<` is shorthand for `<C 10` and `>` is shorthand for `A>`.


### Run-Length Tape Compression

Simulating this TM will have a **lot** of `x`s on the tape. Instead of representing the tape as a sequence of symbols (like `xxxx...x`) you will have to implement run-length encoding for `x`s. In other words, you'll need a way to represent $$x^n$$.


## Transition Rules

We still have not found any way to exhaustively list all rules starting from arbitrary configs in Extended Counter Notation because collisions can be quite complicated depending on the exact details of their surroundings. However, it appears that the following rules are sufficient to simulate this TM for all time (in practice). We came upon this list by a bit of trial and error:

$$ \begin{array}{l}
    &   & x^n & < & \to & < & x^n   &   \\
    &   & D   & < & \to & < & D     &   \\
    &   & C   & < & \to & < & C     &   \\
    & > & x   &   & \to &   & x     & > \\
    & > & D   &   & \to &   & D     & > \\
\\
    & > & R   &   & \to & < & R     &    \\
    && L & < C x & \to &   & L C_1 D & >P \\
\\
  x & > & C   &   & \to &   & C_0   & > \\
    &   & C_0 & < & \to &   & C_1 x & > \\
    &   & C_1 & < & \to &   & C_2   & > \\
    &   & C_2 & < & \to &   & C   x & > \\
\\
  D & > & C   &   & \to &   & P x   & > \\
\\
C_2 & > & C   &   & \to &   & F_0   & > \\
    &   & F_0 & < & \to &   & F_1 x & > \\
    &   & F_1 & < & \to &   & F_2   & > \\
    &   & F_2 & < & \to &   & F_3 x & > \\
    & & x F_3 & < & \to & < & P C D &   \\
\\
C_0 & > & C   &   & \to &   & G_0   & > \\
    &   & G_0 & < & \to &   & G_1 x & > \\
    &   & G_1 & < & \to &   & G_2   & > \\
    &   & G_2 & < & \to &   & P D x & > \\
\\
    &    & P  & < & \to & < & P         &    \\
    & >P & P  &   & \to &   & x         & >  \\
    & >P & x^n &  & \to &   & x^n       & >P \\
    & >P & R  &   & \to & < & C x R     &    \\
    & >P & Dx &   & \to &   & C_1 D     & >P \\
    & >P & Cx &   & \to & < & P D P     &    \\
    & >P & DP &   & \to &   & C_1 D     & >  \\
    & >P & DDx &  & \to &   & C_2 C_1 D & >  \\
    & >P & DCx &  & \to &   & G_1 D     & >P \\
\end{array} $$

You can begin simulating in config $$ L C_1 > R $$ which the TM enters at step 19.

Note that although this list is quite long it is still "incomplete" in the sense that there exist valid Extended Counter Notation configs to which none of these rules apply. One notable example is:

$$ > PDDR \to \text{Halt} $$

But, in practice, it appears that these rules are enough to simulate the TM (starting from blank tape).


## Counter Acceleration

In theory, the rules above are sufficient to simulate this TM until it reaches Translation Cycling given enough time. However, you may not live to see the result. In order to improve the runtime, there are two accelerations rules that we found useful. The first is actually the whole reason for using the "Counter" abstraction listed above. As I mentioned in my original article, this TM is very orderly while it is in "Counter Config" and only becomes chaotic once a "collision" occurs. There are many ways to specify Counter Config rules, but the most useful one I've found is:

$$ \begin{array}{l}
& > & w_0 & x & C &     & w_1 & x^4 & C &     & ... & w_n & x^{4^n} & C &                 & w_{n+1} & R & \to \\
& < & w_0 &   & C & x^2 & w_1 &     & C & x^8 & ... & w_n &         & C & x^{2 \cdot 4^n} & w_{n+1} & R & \\
\end{array} $$

Where all $$
w_i \in (x|D)^*$$.

This can be proven via induction using the rules listed above.

In other words, if the right half of the tape has no `P`s and has "enough" `x`s to the left of each `C`, then you can do an accelerated step across the entire right side and back. I call this a "stride". This is an exponential speedup as long as the TM remains in Counter Config (between collisions).


## @uni-cycle Acceleration

There is only one final acceleration that is necessary to run this TM until it reaches an infinite cycle: The generalized @uni-cycle. I [mentioned]({% post_url 2023-02-25-skelet-1-wip %}#the-uni-cycle) the @uni-cycle in my original post. But since then we made a new discovery, which is that after exiting the original @uni-cycle, this TM re-enters a very similar cycle, and after that one another similar cycle, etc. I had originally thought that we might have to deal with many different flavors of cycles. But it turns out that this TM really likes one flavor!

The basic idea about the generalized @uni-cycle is that there is a parameterized subset of a configuration that whenever the TM gets into it, we can see that it will cycle back to that config repeatedly until the cycle breaks. Everything to the left of the @uni-cycle config can be ignored, the TM will never touch that again. Everything to the right of the @uni-cycle config will be repeatedly have the "stride" rule (from the last section) applied. Each cycle, two things change: One of the run-lengths of `x`s ($$n$$) is reduced by a specific number ($$P = 53\,946$$) and a specific number of "strides" happen to the right side of the tape ($$T = 215\,779$$). The cycle rule can be applied until either (1) $$n < P$$ or (2) we cannot apply $$T$$ stride rules to the right (because it would cause a collision).

The @uni-cycle is ... a cycle, so we can pick any intermediate point in the cycle to label the "start". In my last post I chose an extremely arbitrary point, here I list a slightly lest arbitrary one (immediately after one of the 9 collisions that occur in each cycle), but feel free to choose any point in the cycle that is convenient for you.

Let:

$$\begin{array}{l}
  F & = & C_2 & x^{7640} & D & x^{10\,344} \\
  G & = & x^{300} & D & x^{30\,876} & D & x^{72\,142} & D & x^{3076} & D & x^{1538} & D & \\
  J & = & C_1 & x^{7640} & D & x^{10\,345} & \\
    &   & C_3 & x^{7639} & D & x^{10\,347} & \\
    &   & C_3 & x^{7635} & D & x^{10\,355} & \\
    &   & C_1 & x^{7618} & D & x^{10\,389} & \\
    &   & C_2 & x^{7550} & D & x^{10\,524} & \\
    &   & C_0 & x^{7279} & D & x^{11\,066} & \\
    &   & C_3 & x^{6197} & D & x^{13\,231} & \\
    &   & C_1 & x^{1866} & D &        & D & x^{7713} & \\
    &   & C_0 & x^{95} \\
    &   & C_2 &          & D & \\
\end{array} $$

and

$$\begin{array}{l}
  P & = & 53\,946 \\
  T & = & 4 P - 5 & = & 215\,779 \\
\end{array} $$


For any right side $$
r \in (x|D|C)^* R$$ such that we can apply the "stride" rule $$T$$ times (let $$s^T(r)$$ be the result of doing so) and for any $$n \ge P$$, then:

@uni-cycle Rule:

$$\begin{array}{l}
    & J & x^n     & C_1 & D & > &   & r & \to \\
  F & J & x^{n-P} & C_1 & D & > & G & s^T(r) & \\
\end{array} $$

Note: This rule only matches and rewrites the right half of the tape. It ignores anything to the left of $$J$$ in the initial config. In other words, the rule matches any tape with a "suffix" like this.

This rule can be proven by applying the rules from previous sections. It goes through exactly 9 collisions each cycle.

And the real acceleration occurs when you apply this rule repeatedly. So, if $$y = \lfloor n / P \rfloor$$, $$m$$ is the max number of strides that can be applied to $$r$$ before a collision and $$z = \lfloor m / T \rfloor$$, then we can apply the @uni-cycle rule $$k = min(y, z)$$ time in a single step:

$$\begin{array}{l}
      & J & x^n        & C_1 & D & > &     & r & \to \\
  F^k & J & x^{n - kP} & C_1 & D & > & G^k & s^{kT}(r) & \\
\end{array} $$

Note: That these exponents can get really big. In our simulation we saw examples of $$k > 10^{19}$$. So, you cannot expand $$F^k$$ and $$G^k$$ out directly in your tape representation, you'll need to store that compressed in some way (in our simulator, we added new pseudo tape symbols for $$F$$ and $$G$$ which are also stored with run-length encoding).


## Perpetual Skelet Cycle

Ah, finally. Once you have implemented custom tape encoding, counter accelaration and generalized @uni-cycle acceleration. You must simply run your simulation for 30-ish million simulator steps and you will reach the _Perpetual Skelet Cycle_, a cycle that never ends!

Let

$$\begin{array}{l}
  F & = & C_2 & x^{7640} & D & x^{10\,344} \\
  K & = & C_0 & x^{7639} & D & x^{10\,346} \\
    &   & C_0 & x^{7635} & D & x^{10\,354} \\
    &   & C_0 & x^{7619} & D & x^{10\,386} \\
    &   & C_0 & x^{7555} & D & x^{10\,514} \\
    &   & C_0 & x^{7299} & D & x^{11\,026} \\
    &   & C_0 & x^{6275} & D & x^{13\,074} \\
    &   & C_0 & x^{2179} & D && D & x^{7087} \\
    &   & C_0 & \\
    &   & C_0 & x^{3849} & \\
\end{array} $$

(Note: Interestingly, $$F$$ is the same as in the @uni-cycle.)

Then,

Perpetual Skelet Cycle rule:

$$ K < R \to F K < R $$

As previously, this is only a suffix of the tape. It allows and ignores anything to the left of the $$K$$ in the input config. Specifically, any output config matches the input config and thus it will repeat forever!


## Summary

All said, I think Skelet \#1 is quite a fascinating beast when it comes to analysis of chaotic system. It spends the majority of it's time doing orderly behavior, punctuated by chaotic interludes. Interestingly, we can see that on many different levels:

  1. On the lowest level, if you run this TM directly you will see that it spends basically all of it's time simply moving back and forth across vast fields of `x`s ($$>x \to x>$$ and $$x< \to <x$$). Occasionally, this is punctuated by interactions with dots, clots, etc.
  2. One level up, in the Counter abstraction, you can see that the TM spends basically all of it's time counting up a base-4 structural counter. Occasionally, this orderly behavior is interrupted by a collision leading to comparatively chaotic results.
  3. One level up, this TM spends basically all of it's time cycling through generalized @uni-cycles, sequences of 9 collisions each which form stable cycles somehow finding order from the chaos of the level below. Occasionally, these orderly cycles are interrupted briefly when an exponent runs out and chaos reigns again briefly before a new cycle is quickly formed.
  4. And finally, it enters the perpetual Skelet cycle and we see that all that came before is simply an elaborate setup for this final, completely orderly behavior forever after.

Up until discovering the perpetual Skelet cycle, I wasn't sure if these levels would ever end. It seemed quite possible that every time we abstracted this TM one level higher, there was just some stable cycle that formed for a time, but then broke apart into a chaotic interlude before reforming again at higher and higher and higher levels of abstraction. But no, after 3 levels of abstraction, Skelet \#1 appears to have run out of chaos.


{% include skelet-toc.html %}
