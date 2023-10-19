---
layout: post-custom
title: >
  Skelet #1: A Halting Counter Config
tags: busy-beaver skelet
---

> Update (13 Mar 2023): [Skelet \#1 is infinite ... we think]({% post_url 2023-03-13-skelet-1-infinite %})

In my previous [Skelet \#1 post]({% post_url 2023-02-25-skelet-1-wip %}) I discussed how this TM spends most of it's time in orderly Counter Configurations interrupted occasionally by "collisions" which lead to more chaotic behavior until generally settling back into a Counter Configuration.

One open question after that post was: Are there any Counter Configurations that lead to halting? If we could prove that none could, we have proven Skelet \#1 infinite. If we could find one, then maybe Skelet \#1 has a chance to halt (by eventually reaching such a counter config)!

The next morning, in the shower, I figured out how to construct such a Counter Config that halts! One such config is:

$$ ... C_0 \;\; x^f \;\; D C_2 \;\; x^e \;\; D C_1 \;\; x^d \;\; D C_1 \;\; x^c \;\; D C_2 \;\; x^b \;\; DC \;\; x^a \;\;\;\; < \;\; R $$

for any $$a, b, c, d, e, f \ge 0$$ (all sequences of $$x$$ can be arbitrarily long).

This config does require quite a few coincidences to occur. Notably that we have five different pre-collision $D C_k$ sections lined up and timed perfectly and that there are no extraneous dots ($$D$$) not involved in such collisions. However, given infinite time it seems perfectly reasonable to imagine that the TM will eventually experience such a coincidence!


## Counter Notation

As a reminder, a Counter Configuration is defined as:

$$ (x|D|C_0|C_1|C_2|C_3)^* \; (<|>) \; (x|D|C)^* \; R $$

Where:

| Name | Counter Code | Run-length | Raw Tape (Left) | Raw Tape (Right) |
| :------------- | :---------- | :---- | -------------: | :-------- |
| Filler         | $$x$$       | `22`  |      `011 011` | `110 110` |
| Dot            | $$D$$       | `21`  |       `011 01` | `110 10`  |
| Digit 0        | $$C_0$$     | `232` | `011 0111 011` |           |
| Digit 1        | $$C_1$$     | `201` |     `011 0 01` |           |
| Digit 2        | $$C_2$$     | `42`  |    `01111 011` |           |
| Digit 3 (Clot) | $$C_3 = C$$ | `11`  |        `01 01` | `10 10`   |
| Right End      | $$R$$       | `2$`  |                | `110 $`   |
| Parity Change  | $$P$$       | `2`   |          `011` | `110`     |

And `<` represents `<C 10` and `>` represents `A>`. Note, I will use $$C_3$$ and $$C$$ interchangeably.

Once in a Counter Config, the TM remains there until a "collision" event occurs. Collisions only happen when we have $$a C$$ for $$a \ne x$$ (i.e. $$DC$$ and $$C_k C$$).


## Relevant Rules

The following rules will be necessary to analyze my examples:

Counter Rules:

$$ \begin{array}{l}
    & x   & < & \to & < & x   \\
    & D   & < & \to & < & D   \\
    & C   & < & \to & < & C   \\
    & C_0 & < & \to &   & C_1 x & > \\
    & C_1 & < & \to &   & C_2   & > \\
    & C_2 & < & \to &   & Cx  & > \\
  > & x   &   & \to &   & x   & > \\
  > & D   &   & \to &   & D   & > \\
  > & xC  &   & \to &   & C_0 & > \\
  > & R   &   & \to & < & R
\end{array} $$

Relevant `DC` Collision Rules:

$$ \begin{array}{l}
  >  & DC &   & \to &   & xP    & >  \\
     & P  & < & \to & < & P     &    \\
  >P & P  &   & \to &   & x     & >  \\
  >P & x  &   & \to &   & x     & >P \\
  >P & R  &   & \to & < & CxR   &    \\
  >P & Cx &   & \to & < & PDP   &    \\
  >P & DP &   & \to &   & C_1 D & >  \\
  >P & DDR &  & \to & \mbox{**Halt**} \\
  >P & Dx &   & \to &   & C_1 D & >P \\
\end{array} $$


## Proof of Halting

So, without further ado, Ie can now demonstrate how the Counter Config I listed above leads to a halt:

$$ \begin{array}{l}
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_1 & x^c & D C_2 & x^b & DC & x^a & < & R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_1 & x^c & D C_2 < & x^b & DC & x^a &   & R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_1 & x^c & DCx & x^b & xP & x^a & > & R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_1 < & x^c & DCx & x^b & xP & x^a & & R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_2 & x^c & xPx & x^b & x & x^a & >P & R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_2 & x^c & xPx & x^b & x & x^a & < & CxR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & D C_2 < & x^c & xPx & x^b & x & x^a &  & CxR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & DCx & x^c & xx & x^b & x & x^a & >P & CxR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 & x^d & DCx & x^c & xx & x^b & x & x^a & < & PDPR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_1 < & x^d & DCx & x^c & xx & x^b & x & x^a & & PDPR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_2 & x^d & xPx & x^c & xx & x^b & x & x^a & > & PDPR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_2 & x^d & xPx & x^c & xx & x^b & x & x^a & & C_1 D > R & \to \\
  C_0 & x^f & D C_2 & x^e & D C_2 & x^d & xPx & x^c & xx & x^b & x & x^a & < & CxDR & \to \\
  C_0 & x^f & D C_2 & x^e & D C_2 < & x^d & xPx & x^c & xx & x^b & x & x^a & & CxDR & \to \\
  C_0 & x^f & D C_2 & x^e & DCx & x^d & xx & x^c & xx & x^b & x & x^a & >P & CxDR & \to \\
  C_0 & x^f & D C_2 & x^e & DCx & x^d & xx & x^c & xx & x^b & x & x^a & < & PDPDR & \to \\
  C_0 & x^f & D C_2 < & x^e & DCx & x^d & xx & x^c & xx & x^b & x & x^a & & PDPDR & \to \\
  C_0 & x^f & DCx & x^e & xPx & x^d & xx & x^c & xx & x^b & x & x^a & > & PDPDR & \to \\
  C_0 & x^f & DCx & x^e & xPx & x^d & xx & x^c & xx & x^b & x & x^a & & C_1 D > DR & \to \\
  C_0 & x^f & DCx & x^e & xPx & x^d & xx & x^c & xx & x^b & x & x^a & < & CxDDR & \to \\
  C_0 < & x^f & DCx & x^e & xPx & x^d & xx & x^c & xx & x^b & x & x^a & & CxDDR & \to \\
  C_1 x & x^f & xPx & x^e & xx & x^d & xx & x^c & xx & x^b & x & x^a & >P & CxDDR & \to \\
  C_1 x & x^f & xPx & x^e & xx & x^d & xx & x^c & xx & x^b & x & x^a & < & PDPDDR & \to \\
  C_1 x < & x^f & xPx & x^e & xx & x^d & xx & x^c & xx & x^b & x & x^a & & PDPDDR & \to \\
  C_2 x & x^f & xx & x^e & xx & x^d & xx & x^c & xx & x^b & x & x^a & >P & PDPDDR & \to \\
  C_2 x & x^f & xx & x^e & xx & x^d & xx & x^c & xx & x^b & x & x^a & & xD >P DDR & \to \\
  \mbox{**Halt**}
\end{array} $$


## Theory

I did not just happen upon this halting counter config by chance, nor through brute force search. Instead I was specifically searching for a config that would deliver a "halting signal sequence" as I described in my last post. Specifically, look at the analysis above and consider the left and right sides of the tape separately where the left is most of the config (from $$x^a$$ to the far left) and the right side starts as just $$R$$ and ends as $$xD > PDDR$$. Splitting it up this way we can see that the right side can be led to halt via the following sequence:

$$ \begin{array}{llr}
  & >P & R      & \to & < & CxR \\
  & >P & CxR    & \to & < & PDPR \\
  & >  & PDPR   & \to & < & CxDR \\
  & >P & CxDR   & \to & < & PDPDR \\
  & >  & PDPDR  & \to & < & CxDDR \\
  & >P & CxDDR  & \to & < & PDPDDR \\
  & >P & PDPDDR & \to &   & \mbox{**Halt**} \\
\end{array} $$

I call this sequence of `>` and `>P` approaches a "signal sequence". Using `a` for `>` (aligned) and `P` for `>P` (parity change) I call this sequence `PPaPaPP`. So, any left-hand side that produces this precise signal sequence will lead to a halt.

My shower revelation was that we can hand-craft a counter config (left side) to match any specific signal sequence using components:

  * $$D C_2 \; x^*$$ for `P`
  * $$D C_1 \; x^*$$ for `aP`
  * $$D C_0 \; x^*$$ for `aaP`
  * ... I think `aaaP` etc. should also be constructible pretty easily if you needed to.

sequentially (from right-to-left). So `PPaPaPP = P P aP aP P` can be built as:

$$ \begin{array}{r}
  D C_2 & x^* & : & (P)  \\
  D C_2 & x^* & : & (P)  \\
  D C_1 & x^* & : & (aP) \\
  D C_1 & x^* & : & (aP) \\
  D C_2 & x^* & : & (P)  \\
\end{array} $$

which combine to

$$ D C_2 \;\; x^* \;\; D C_1 \;\; x^* \;\; D C_1 \;\; x^* \;\; D C_2 \;\; x^* \;\; D C_2 \;\; x^* $$

Which is basically the config I listed.


## Simpler Halting Config

We can simply the config needed to reach halt a bit further to:

$$ ... \;\; D C_1 \;\; w \;\; D \;\; x^c \;\; D C_2 \;\; x^b \;\; DC \;\; x^a \;\;\;\; < R $$

where $$
w \in (x|D)^*$$.

In other words, I think we only need 3 $$D C_k$$ coincidences and only need to exclude $$D$$s in the rightmost two segments ($$x^a$$ and $$x^b$$).

I leave this as an exercise to the reader. But the basic insight here is that of the `PPaPaPP` halting signal sequence only the `PPaPa` part is critical. After that signal sequence, the right side ends in $$DDR$$ and since $$ > DDR \to < DDR $$ and $$ >P DDR \to \mbox{**Halt**}$$, halting is basically inevitable!


{% include skelet-toc.html %}
