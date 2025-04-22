---
layout: post-custom
title: >
  Lucy's Moonlight: The 5% Champion
tags: busy-beaver collatz cryptid
---

There is a 5% chance that we have a new BB(6) champion! It's a machine discovered by Racheline which she's named "Lucy's Moonlight". It is a [probviously](https://wiki.bbchallenge.org/wiki/Probvious) halting tetrational [Cryptid](https://wiki.bbchallenge.org/wiki/Cryptids). In other words, it's a machine that we believe will eventually halt (with 100% chance via a probabilistic approximation), yet proving it requires either solving a Collatz-like problem or simulating it for a "tetrational" number of steps. This is very similar to the [Mother of Giants]({% post_url 2022-04-03-mother-of-giants %}) with the exception that if the Mother of Giants TMs (quasi-)halt, then they are known to beat the current BBB(5) champion, whereas if Lucy's Moonlight halts there is only a 5% chance that it beats the current [10↑↑15 BB(6) champion]({% post_url 2022-06-21-bb-6-2-t15 %}), in the other 95% cases it will halt in fewer steps.


## Machine

The machine is `1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_1RZ0RA`:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 0RD |
|  B  | 0RC | 1RE |
|  C  | 1RD | 0LA |
|  D  | 1LE | 1LC |
|  E  | 1RF | 0LD |
|  F  | 1RZ | 0RA |

It was discovered by Racheline on 1 Mar 2025 on the bbchallenge Discord ([Discord link](https://discord.com/channels/960643023006490684/1239205785913790465/1345551751016878272)) and has bbchallenge wiki page: [Lucy's Moonlight](https://wiki.bbchallenge.org/wiki/Lucy%27s_Moonlight).


## Analysis

Let

$$ C(a, b) = 0^\infty \;\; 1011^a \;\; 1^b \;\; 10 \;\; \text{ C> } \;\; 0^\infty $$

then, it can be proven that

$$ \begin{array}{l}
  \text{Start} & & \xrightarrow{2} & C(0, 0) \\
  \\
  C(a+1, & 3k)   & \to & C(a, 8k+6) \\
  C(a+2, & 3k+1) & \to & C(a, 8k+16) \\
  C(a+2, & 3k+2) & \to & C(a, 8k+22) \\
  \\
  C(0,   & 3k+1) & \to & C(0, 8k+5) \\
  C(0,   & 3k+2) & \to & C(0, 8k+5) \\
  \\
  C(0,   & 3k)   & \to & C(2k,   8) \\
  C(1,   & 3k+2) & \to & C(2k+4, 8) \\
  \\
  C(1,   & 3k+1) & \to & \text{Halt}(6k+14) \\
\end{array} $$

Where, as usual, these rules apply for all non-negative integer variable assignments ($$a,k \ge 0$$) and Halt(n) means that the TM halts with $$n$$ non-zero symbols on the tape.

I've split these rules up into blocks to emphasize the different phases or types of rules:

Main Phase:

$$ \begin{array}{l}
  C(a+1, & 3k)   & \to & C(a, 8k+6) \\
  C(a+2, & 3k+1) & \to & C(a, 8k+16) \\
  C(a+2, & 3k+2) & \to & C(a, 8k+22) \\
\end{array} $$

This repeatedly applies a Collatz-like iteration, reducing $$a$$ at each step until $$a$$ is too small.

Reset Rules:

$$ \begin{array}{l}
  C(0,   & 3k)   & \to & C(2k,   8) \\
  C(1,   & 3k+2) & \to & C(2k+4, 8) \\
\end{array} $$

If $$a,b$$ match either of these two patterns then the TM resets back to a new main phase.

Zero Phase:

$$ \begin{array}{l}
  C(0,   & 3k+1) & \to & C(0, 8k+5) \\
  C(0,   & 3k+2) & \to & C(0, 8k+5) \\
\end{array} $$

If $$a = 0$$ and $$3 \nmid b$$ then the TM iterates a different Collatz-like function until it reaches a config where $$3 \mid b$$ and then resets.

Halt Rule:

$$ \begin{array}{l}
  C(1,   & 3k+1) & \to & \text{Halt}(6k+14) \\
\end{array} $$

Finally, if it hits this specific configuration, the TM halts.


## Trajectory

Note that after each reset, we end up in a config $$C(a, 8)$$. Let's call each of these configs "checkpoints". Following each checkpoint, the Lucy's Moonlight will either Halt, Reset or (theoretically) get stuck forever iterating Zero Phase.

Starting from a blank tape, the TM enters config $$C(0,0)$$ after 2 steps and then follows the trajectory:

$$ \begin{array}{l}
  C(0, 0) & \to & C(    0, 8) & \to & C(0, 21) \\
          & \to & C(   14, 8) & \to & \cdots & \to & C(1, 16934) \\
          & \to & C(11292, 8) & \to & \cdots & \to & C(0, \approx 10^{2902.09}) \\
          & \to & C(\approx 10^{2901.92}, 8) & \to & \cdots \\
\end{array} $$

So, the first four checkpoints occur with $$a$$ values:

$$ \begin{array}{lcr}
  c_0 & = & 0 \\
  c_1 & = & 14 \\
  c_2 & = & 11\,292 \\
  c_3 & \approx & 10^{2901.92} \\
\end{array} $$

<details>
  <summary>Exact value for c3</summary>
  8 282 581 182 265 963 777 660 116 067 041 084 396 825 871 729 769 475 063 015 437 507 606 888 488 657 640 984 741 410 755 868 651 202 413 557 949 100 792 150 345 468 805 096 096 950 985 621 014 344 543 514 277 244 259 988 659 130 143 328 155 990 590 108 709 713 794 583 253 323 686 355 356 512 219 061 229 636 197 885 927 258 835 226 571 319 297 308 352 230 934 484 006 197 639 625 592 087 971 234 386 001 742 614 119 317 946 288 524 516 349 575 343 597 522 485 283 906 000 542 032 088 582 043 646 950 532 639 630 385 985 319 217 159 379 913 000 142 879 141 905 099 969 565 530 694 702 807 960 713 276 894 845 659 927 803 312 770 155 623 893 892 127 451 599 296 902 730 174 376 009 710 735 758 161 389 656 270 797 836 582 256 488 031 353 066 716 635 172 987 950 448 854 471 226 597 449 927 236 184 172 841 640 111 209 332 317 049 722 869 659 569 874 196 714 141 959 835 401 796 418 444 068 891 026 981 841 656 732 128 708 017 637 486 218 786 090 173 524 036 425 924 718 502 564 851 924 717 340 390 259 248 282 032 112 075 387 681 859 362 344 399 913 313 735 645 684 525 131 229 468 282 784 360 728 881 748 147 372 112 747 036 418 378 308 364 410 128 040 328 676 209 420 026 633 482 346 143 509 117 105 276 670 245 493 297 604 407 287 182 199 289 609 254 900 080 171 095 368 953 306 931 467 191 729 590 199 363 109 109 618 828 683 456 945 716 771 345 293 252 204 756 902 270 830 478 266 505 243 340 324 828 877 091 406 917 371 244 363 787 314 164 920 400 219 556 757 173 398 748 668 149 395 792 060 530 400 633 872 912 079 249 392 256 126 285 748 793 796 259 657 854 699 829 517 626 609 309 417 076 213 461 174 150 922 612 299 942 658 509 909 739 815 101 078 137 303 456 289 178 147 820 849 027 886 955 738 533 503 625 157 087 287 391 831 669 455 397 075 444 062 908 165 633 623 616 230 849 011 917 173 994 535 718 598 409 770 737 638 239 724 998 256 861 644 166 630 982 723 063 781 225 891 358 955 048 633 229 232 741 851 699 498 876 266 677 026 907 578 098 686 784 323 407 335 765 343 701 077 746 445 802 114 304 942 791 377 887 303 588 107 097 550 867 703 017 440 867 027 391 593 474 985 628 594 939 344 739 091 341 577 631 399 711 011 114 031 231 392 355 268 858 286 239 590 222 739 798 802 836 719 470 359 138 334 346 097 704 505 528 574 751 020 940 898 407 003 617 333 219 550 156 008 932 231 022 648 658 161 473 903 774 681 072 952 056 320 551 244 912 271 864 381 014 835 634 282 966 523 463 985 953 949 176 576 786 408 020 337 836 220 233 538 960 379 978 664 849 564 796 907 967 238 406 785 655 383 464 715 057 232 716 549 617 630 853 473 306 701 852 904 885 606 863 527 445 121 198 511 795 947 547 473 084 736 520 465 867 328 457 114 967 373 145 664 842 684 299 186 357 472 668 934 663 435 131 859 827 025 004 311 417 517 867 339 721 854 899 421 107 496 540 906 158 691 502 108 225 108 606 878 323 722 157 711 420 937 080 640 504 487 833 073 951 122 223 191 857 376 281 164 771 129 574 147 051 021 933 227 085 689 827 336 672 385 780 462 357 440 219 398 403 103 894 053 177 742 661 833 002 598 347 813 276 596 599 779 500 262 101 470 481 321 560 131 349 255 581 918 937 061 811 724 247 415 920 101 784 002 187 650 019 023 113 164 778 491 907 065 101 691 880 663 970 914 566 257 787 660 257 139 627 941 284 081 248 034 057 979 564 419 991 961 345 637 080 391 063 343 278 659 875 006 722 945 435 258 511 750 736 395 209 297 602 991 695 886 580 794 249 759 902 536 380 055 710 080 977 728 337 952 498 718 259 606 078 790 355 738 625 858 597 516 649 364 990 083 397 742 948 406 390 695 711 840 139 170 928 984 527 396 432 236 103 181 667 418 006 635 667 189 873 871 634 905 950 683 958 299 923 219 653 264 060 399 588 571 782 767 511 747 924 043 969 623 045 308 763 567 170 166 295 093 013 227 497 346 173 854 101 964 306 147 690 034 349 284 712 163 842 269 859 435 320 408 715 901 523 168 064 489 459 605 434 861 066 611 056 209 626 985 578 507 010 825 633 753 454
</details>
&nbsp;

At this point it would require simulating more than $$10^{2901}$$ iterations of Main Phase in order to compute what happens next.


## Probabilistic Argument

As with previous Collatz-like TMs, we will now consider a probabilistic model where we basically assume that the Collatz iterations operate like a (pseudo-)random number generator which has an equal chance of choosing any remainder modulo 3 for $$b$$ at each step.

So,
* When $$a \ge 2$$, there will be a 1/3 chance of reducing $$a$$ by 1, and 2/3 chance of reducing $$a$$ by 2.
* When $$a = 1$$, there is a 1/3 chance of each: Halting immediately, resetting, entering Zero phase.
* When $$a = 0$$, Zero Phase will always eventually end, leading to a reset.

In this model, after every checkpoint, there are only two options: reset or halt. Probability of halt is 1/3 times the probability that we reach the $$a = 1$$ case (i.e. that it wasn't skipped by a Main Phase jump). Let $$P(n)$$ be the probability that starting from some config $$C(a, b)$$ (for any $$a>n$$) it reaches $$C(a-n, b^\prime)$$ (via Main Phase rules alone). Well, the only way for this iteration to not reach $$C(a-n, b^\prime)$$ is if it skipped over this $$a$$ value. In other words:

$$ P(n) = 1 - \frac{2}{3} P(n-1) $$

This recurrence has its only fixed point at $$\frac{3}{5}$$ and if $$P(n) = \frac{3}{5} + \epsilon$$ then $$P(n+1) = \frac{3}{5} - \frac{2}{3} \epsilon$$ so the probabilities approach this fixed point exponentially quickly: $$
|P(n) - \frac{3}{5}| = \frac{2}{5} \left(\frac{2}{3}\right)^n$$. This is already tiny by $$n=20$$ ($$\left|P(14) - \frac{3}{5}\right| < 0.0002$$), by the time you get to $$n \approx 10^{2901.92}$$ the discrepancy is minute.

Therefore, starting at any large checkpoint, there is a $$\frac{1}{5}$$ chance of halting this iteration ($$\frac{3}{5}$$ chance of reaching $$a=1$$ and $$\frac{1}{3}$$ chance to halt if reaching $$a=1$$) and a $$\frac{4}{5}$$ chance of resetting. And thus, the chance that it resets at least $$r$$ times starting from a given checkpoint is $$\left(\frac{4}{5}\right)^r$$


### Champion Chance

How many resets do we need to beat the current champion?

First note that both the reset rules produce a checkpoint config with even first parameter. So, without loss of generality, we can consider two sequential checkpoints as:

$$ C(2 a_n, 8) \to C(2 a_{n+1}, 8) $$

In order to reset we know that this included **at least** $$a_n$$ iterations of Main Phase and so

$$ a_{n+1} \ge \frac{8}{3} \left(\frac{8}{3}\right)^{a_n} $$

If $$ C(2 a_n, 8) \to Halt(x) $$ then there is similar growth:

$$ x \ge 16 \left(\frac{8}{3}\right)^{a_n} $$

If we let $$b_k = \frac{3}{8} a_k$$ then

$$ b_{n+1} \ge \left(\frac{8}{3}\right)^{\frac{8}{3} b_n} > 10^{b_n} $$

Starting from $$C(c_3, 8)$$ with $$c_3 > 8 \times 10^{2901}$$ we get $$b_3 = \frac{3}{16} c_3 > 10^{2901} > 10 \uparrow\uparrow 2.5$$ (using [fractional up-arrow notation]({% post_url 2022-06-25-ext-up-notation %})). If this is followed by $$r$$ resets, then we will reach

$$ b_{r+3} > 10 \uparrow\uparrow (r+2.5) $$

and when it halts there will be $$> 10 \uparrow\uparrow (r+3.5)$$ ones on the tape. The current champion writes $$< 10 \uparrow\uparrow 15.60466$$ ones, so if $$r \ge 13$$, Lucy's Moonlight will beat it.[^precise-resets]

Therefore, in this probabilistic model, knowing that it reaches checkpoint $$C(> 8 \times 10^{2901}, 8)$$, the chance that Lucy's Moonlight will write more ones than than Pavel's current champion is:

$$ \left(\frac{4}{5}\right)^{13} \approx 5.50\% $$

If we could somehow accelerate the simulation and demonstrate that it reset at least once more, then that probability would increase slightly to:

$$ \left(\frac{4}{5}\right)^{12} \approx 6.87\% $$


## Footnotes
[^precise-resets]: It appears that $$r=12$$ resets followed by a halt will not be quite enough to snatch the championship, since it would have $$\approx 10 \uparrow\uparrow 15.54$$ ones, just shy of the $$10 \uparrow\uparrow 15.60466$$ needed for victory. But you'd have to be a little more careful with the analysis to ensure this is true.