---
layout: post
title: BBB Search Process
tag: busy-beaver
---

A few weeks ago, [I announced new Beeping Busy Beaver results]({% post_url 2022-02-17-bbb-5-2-search-results). In this post, I'd like to explain a little bit about the process I'm using to do this search. I'm following roughly the same process that has been used to exhaustively search for standard Busy Beavers in the past with a few tweaks for the Beeping halt condition.

Beeping Busy Beavers are much more complicated that traditional Busy Beavers. In general it is undecidable to even detect that a Beeping Busy Beaver has already quasihalted! So the prospect of trying to actually prove any BBB values is quite daunting! My goal here is considerably humbler: simply to set a baseline, using existing Busy Beaver techniques, of which Turing Machines (TMs) we can conclusively categorize as Quasihalting and Infinite non-quasihalting and how many remain Unknown.

## Process Overview

Here's an outline of the process I am using to search for BBB:

1. Enumerate all TMs in Tree Normal Form (TNF, [Nick Drozd calls this Brady's algorithm](https://nickdrozd.github.io/2022/01/14/bradys-algorithm.html)) expanded to allow TMs with 0 halt transitions.

2. Run the [Lin Recurrence algorithm](https://nickdrozd.github.io/2021/02/24/lin-recurrence-and-lins-algorithm.html) first on each to catch simple recurrent TMs early.

3. Run our Accelerated Simulator which uses Heiner Marxen's [Macro Machines](http://turbotm.de/~heiner/BB/macro.html) and rule proving mechanism (Marxen calls these [CTRs (configuration transitions)](http://turbotm.de/~heiner/BB/bbsimtab.html)).

The results is that all TMs are classified as: Halting, Quasihalting, Infinite non-quasihalting or Unknown.


## Quasihalting Behavior

As I mentioned above, we are only scratching the surface when it comes to detecting quasihalting. Instead of detecting quasihalting in general, I am really only detecting certain types of quasihalting behavior. I have augmented some parts of our system that previously detected non-halting behavior to also classify the TMs as quasihalting or not.

Specifically, I have augmented 3 parts: Lin Recurrence Detection, Simulator Chain Steps and Proof System Rules (PA-CTRs). These 3 methods were originally used to prove that a TM would not halt, but in reality they give us a way to know exactly what configuration the TM will be in at any step in the future. In fact, all three demonstrate different complexities of infinite recurrence. If there exists a state `Q` not used within that recurrence then we can conclude that this TM quasihalts with respect to state `Q`.

### Chain Recurrence

The simplest type of infinite recurrence is a type I will call "Chain Recurrence". This is what happens when you apply a [Chain Step]({% post_url 2021-07-17-bb-collatz %}#chain-step) to one of the infinite blocks of 0s. For example, consider the (2x4 champion I listed above) at step 205,770,076,433,044,242,247,860:

<samp>
0<sup>∞</sup> <b><B</b> 1 2<sup>2</sup> 1<sup>13</sup> 2<sup>414095476532</sup> 0<sup>∞</sup>
</samp>

Since this TM has transition `B0 → 0LB` we can see that this TM will now simply move to the left forever staying continuously in state `B` looking at symbol `0`. Therefore it quasihalts with respect to state `A`.

Chain Recurrence is the easiest way to detect quasihalting. All you need is a simulator that has [Tape Compression]({% post_url 2021-07-17-bb-collatz %}#tape-compression) and performs Chain Steps.

Note: When combined with [Macro Machines](http://turbotm.de/~heiner/BB/macro.html), it is possible to have a TM which enters Chain Recurrence, but does not Quasihalt. Consider the following machine for example:

... TODO: `1RB ---  1RA ---`

### Lin Recurrence

The second simplest form of recurrence is "Lin Recurrence". This is a type of behavior first described by Shen Lin in his [PhD dissertation in 1963](http://rave.ohiolink.edu/etdc/view?acc_num=osu1486554418657614). Lin called it "partial recurrence", but recently Nick Drozd has extensively used the term "Lin Recurrence" to describe it and I will follow suit.

In order to describe partial recurrence, I will first define what I'll call "complete recurrence". If a TM is in a specific configuration (tape & state) and then after `n` steps it is in an identical configuration, this is complete recurrence and it will obviously repeat forever with period `n` (or more precisely the period is the smallest `n` for which this holds). For example:

TM: `1RB 0RB 3LB 2LB 2LA  2LA 1LB 3RB 4RA 4RB`

starting at step 14138899753662763639604 is in configuration

<samp>
0<sup>∞</sup> 1 <b>(B4)</b> 1 2<sup>172312766452</sup> 0<sup>∞</sup><br>
0<sup>∞</sup> 1 4 <b>(B1)</b> 2<sup>172312766452</sup> 0<sup>∞</sup><br>
0<sup>∞</sup> 1 <b>(B4)</b> 1 2<sup>172312766452</sup> 0<sup>∞</sup><br>
</samp>

so we can see this will repeat forever (with period 2). Note that the entire tape is not actually relevant to this recurrence, in fact only 2 symbols are relevant. If this TM got into any configuration like:

<samp>
... <b>(B4)</b> 1 ...<br>
... 4 <b>(B1)</b> ...<br>
... <b>(B4)</b> 1 ...<br>
</samp>

it would repeat forever (with period 2). This is the essence of partial/Lin recurrence: Ignoring the parts of the tape that are irrelevant to the recurrence. An example of Lin Recurrence is:

TM: `1RB 1RB  1LC 0RB  0LA 1LA`

<samp>
0: ... (B1) 1 1 0<sup>∞</sup><br>
1: ... 0 (B1) 1 0<sup>∞</sup><br>
2: ... 0 0 (B1) 0<sup>∞</sup><br>
3: ... 0 0 0 (B0) 0<sup>∞</sup><br>
4: ... 0 0 (C0) 1 0<sup>∞</sup><br>
5: ... 0 (A0) 0 1 0<sup>∞</sup><br>
6: ... 0 1 (B0) 1 0<sup>∞</sup><br>
7: ... 0 (C1) 1 1 0<sup>∞</sup><br>
8: ... (A0) 1 1 1 0<sup>∞</sup><br>
9: ... 1 (B1) 1 1 0<sup>∞</sup><br>
</samp>

so we can see that once it enters this partial configuration: `... (B1) 1 1 0<sup>∞</sup>`, it will repeat forever (with period 9) because after 9 steps the configuration once again matches the initial config (simply adding a `1` to the `...` section).

Lin Recurrence is an extremely common behavior for small TMs. Lin used this technique to prove non-halting behavior for all but 40 holdouts in the 3x2 Busy Beaver. Furthermore, 93% of 5x2 TMs I enumerated were proven non-halting using Lin Recurrence detection (with recurrence start time + period < 100). It is also worth noting that Chain Recurrence is actually the special case of Lin Recurrence with period 1.

# PA-CTR Recurrence
