---
layout: post
title: BBB Search Process
tag: busy-beaver
---

A few weeks ago, [I announced new Beeping Busy Beaver results]({% post_url 2022-02-17-bbb-5-2-search-results%}). In this post, I'd like to explain a little bit about the process I'm using to do this search. I'm following roughly the same process that has been used to exhaustively search for standard Busy Beavers (see ex: Marxen and Buntrock, [*Attacking the Busy Beaver 5*](http://turbotm.de/~heiner/BB/mabu90.html)) in the past with a few tweaks for the Beeping halt condition.

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

The simplest type of infinite recurrence is a type I will call "Chain Recurrence". This is what happens when you apply a [Chain Step]({% post_url 2021-07-17-bb-collatz %}#chain-step) to one of the infinite blocks of 0s. For example, consider the `BBB(2,4)` champion:

|     |  0  |  1  |  2  |  3  |
| :-: | :-: | :-: | :-: | :-: |
|  A  | 1RB | 2LA | 1RA | 1LB |
|  B  | 0LB | 2RB | 3RB | 1LA |

At step 205,770,076,433,044,242,247,860 it is in configuration:

<samp>
0<sup>∞</sup> <b><B</b> 1 2<sup>2</sup> 1<sup>13</sup> 2<sup>414095476532</sup> 0<sup>∞</sup>
</samp>

Since this TM has transition `B0 → 0LB` we can see that this TM will now simply move to the left forever staying continuously in state `B` looking at symbol `0`. Therefore it quasihalts with respect to state `A`.

Chain Recurrence is the easiest way to detect quasihalting. All you need is a simulator that has [Tape Compression]({% post_url 2021-07-17-bb-collatz %}#tape-compression) and performs Chain Steps.

### Lin Recurrence

The second simplest form of recurrence is "Lin Recurrence". This is a type of behavior first described by Shen Lin in his [PhD dissertation in 1963](http://rave.ohiolink.edu/etdc/view?acc_num=osu1486554418657614). Lin called it "partial recurrence", but recently Nick Drozd has extensively used the term "Lin Recurrence" to describe it and I will follow suit.

In order to describe partial recurrence, I will first define what I'll call "complete recurrence". If a TM is in a specific configuration (tape & state) and then after `n` steps it is in an identical configuration, this is complete recurrence and it will obviously repeat forever with period `n` (or more precisely the period is the smallest `n` for which this holds). For example, consider this TM:

|     |  0  |  1  |  2  |  3  |  4  |
| :-: | :-: | :-: | :-: | :-: | :-: |
|  A  | 1RB | 0RB | 3LB | 2LB | 2LA |
|  B  | 2LA | 1LB | 3RB | 4RA | 4RB |

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

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1RB |
|  B  | 1LC | 0RB |
|  C  | 0LA | 1LA |

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

so we can see that once it enters this partial configuration: <code>... (B1) 1 1 0<sup>∞</sup></code>, it will repeat forever (with period 9) because after 9 steps the configuration once again matches the initial config (simply adding a `1` to the `...` section).

Lin Recurrence is an extremely common behavior (at least for small TMs). Lin used this technique to prove non-halting behavior for all but 40 holdouts in BB(3, 2). Furthermore, in my [BBB(5,2) search]({% post_url 2022-02-17-bbb-5-2-search-results%}) I enumerated 687,161,765 TMs (in TNF) and 602,562,873 (93%!) of them enter Lin Recurrence within less than 100 steps (recurrence start time + period < 100).

It is also worth noting that Chain Recurrence is actually the same thing as Lin Recurrence with period 1. So you could consider Lin Recurrence to be an extension of Chain Recurrence or Chain Recurrence to be a special-case of Lin Recurrence.

### Proof System (PA-CTR) Recurrence

Finally, the most complicated form of recurrence is one that I will call "PA-CTR Recurrence" or "Proof System Recurrence". This is one special case of a technique that I first learned from Heiner Marxen through personal communication in 2006 and 2008. I believe he calls the technique "pure additive configuration transitions" (PA-CTRs) and you can see examples on his [Busy Beaver Simulations](http://turbotm.de/~heiner/BB/bbsimtab.html) page. I've also previously described this technique calling it [Rule Steps]({% post_url 2021-07-17-bb-collatz %}#rule-step).

A Proof System Rule is a transition rule which applies to an entire class of tape configurations. In general, our rules are of the form (starting configuration using tape compression with some variables in expontents) → (ending configuration using tape compression with exponents as functions of variables in start config) in (steps as a function of variables). For example, in my [recent post about the BBB(3,3) champion]({% post_url 2022-02-27-bb-recurrence-relations %}) I proved many rules starting with:

<code>0<sup>∞</sup> <b><C</b> 0<sup>a</sup> 2<sup>b+1</sup> ...</code> → <code>0<sup>∞</sup> <b><C</b> 0<sup>a+2</sup> 2<sup>b</sup> ...</code> in `2a + 3` steps.

Note that the common convention for this rule notation is that it applies for all values of all variables `≥0`. In other words, this rule applies if there are any number (or no) `0`s followed by at least 1 `2` on the right half-tape.

This rule is a common type of rule that we can prove. Specifically, it is a "pure additive configuration transition" (PA-CTR) in Marxen's nomenclature. The "pure additive" part means that the configuration before and after are the same except that some exponents change by adding (or subtracting) a constant value. In this case, `a → a + 2` and `b + 1 → b`. PA-CTRs have the advantage that we can accelerate repeated application down into a single simulator step. See, ex: "Rule 1x" and "Rule 2x" in the previously mentioned BBB(3,3) champion article.

But even simpler is the situation where a PA-CTR increases all exponents. In that case, it will apply again and again and again and never stop. Effectively, this machine has entered into a recurrence as detected by PA-CTR Rule Steps. An example is the [third machine]({% post_url 2022-02-17-bbb-5-2-search-results %}#a-machine-with-slightly-more-complex-quasihalting-behavior) I listed in my BBB(5,2) announcement:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1RD |
|  B  | 1LC | 1LB |
|  C  | 1LD | 1RA |
|  D  | 0RE | 0RD |
|  E  | 1LB | 1RE |

at step 12250514892052349453616935044134 (>1.2 × 10<sup>31</sup>) it is on a blank tape in state D:

<code>0<sup>∞</sup> <D 0<sup>∞</sup></code>

and for any `n ≥ 0` we can prove the following PA-CTR rule:

<samp>
  0<sup>∞</sup> <D 1<sup>n</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> E> 1<sup>n</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> 1<sup>n</sup> E> 0<sup>∞</sup><br/>
  0<sup>∞</sup> 1<sup>n</sup> <B 1 0<sup>∞</sup><br/>
  0<sup>∞</sup> <B 1<sup>n+1</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> <C 1<sup>n+2</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> <D 1<sup>n+3</sup> 0<sup>∞</sup><br/>
</samp>

so: <code>0<sup>∞</sup> <D 1<sup>n</sup> 0<sup>∞</sup></code> → <code>0<sup>∞</sup> <D 1<sup>n+3</sup> 0<sup>∞</sup></code>

Since all of the exponents (all one of them) increase, the rule applies again and again and again ad infinitum and so we can be assure that this machine will recur through this series of 6 simulator steps (a mixture of basic steps and Chain Steps) repeatedly.

It is worth noting that all Lin Recurrent programs are PA-CTR Recurrent (as long as you broaden the idea of PA-CTR Recurrence tape compression with blocks of symbols having an exponent). For example, in the machine 3x2 machine I used to demonstrate Lin Recurrence above, we could rewrite the rule as:

<code>... 1<sup>n</sup> B> 1^2 0<sup>∞</sup></code> → <code>... 1<sup>n+1</sup> B> 1^2 0<sup>∞</sup></code>

In fact, Lin Recurrent programs are exactly the programs that are PA-CTR Recurrent with a rule of the form:

* <code>... X<sup>n</sup> Y S> Z 0<sup>∞</sup></code> → <code>... X<sup>n+1</sup> Y S> Z 0<sup>∞</sup></code> or
* <code>0<sup>∞</sup> Z <S Y X<sup>n</sup> ...</code> → <code>0<sup>∞</sup> Z <S Y X<sup>n+1</sup> ...</code>

Where `X,Y,Z` are all sequences (possibly empty) of symbols. (In our example `X = 1`, `Y` is empty and `Z = 11`).

## Future Work

These are the techniques that we have access to already and have adapted well to the BBB search. But there is a huge world out there of BBB behavior that is not being captured here. Some ideas I have on how to extend this work are:

1. Meta Rules. The way I have defined rules here, each step in the rule must be a basic step or a Chain Step. But what about a rule which uses a previous Rule Step in it's proof? I have explored this to some extent (called "recursive" rules or General_Rules in the codebase) and they turned out to be necessary for analysis of [Pavel Kropitz's current `BB(6, 2)` record holder](https://webusers.imj-prg.fr/~pascal.michel/ha.html#tm62).
2. "Closed Tape Language" (CTL). This is another technique that Heiner explained to me through personal communication. It involves defining a group of configuration transitions and showing that any machine which enters this group will forever remain in it. In our system, we have defined CTLs for a few simple classes of Regular Expressions for defining these groups. CTL does not tell you exactly what configuration the TM will be in at every future step, but instead it tells you that at all future steps it will be in this group of configs. For the original Busy Beaver, all you need to know is that none of the configs in the group are halted, and then you know the machine will run forever. For Beeping Busy Beaver, this could potentially be augmented to prove that a TM will never enter a given state again after some point. But it would probably not be possible to use it to prove that the remaining states will each be reached infinitely often (In other words, we could potentially prove that this TM has quasihalted with respect to state `B` ... but not that it might sometime later quasihalt with respect to state `C`).
3. "Backwards reasoning". This is the idea Heiner Marxen talks about of reasoning backwards from the theoretical point where a TM halts in order to prove that it cannot be reached. For Quasihalting this sounds much harder and perhaps impossible, but as Nick recently pointed out in an email, it could be used to show that a TM will never enter Chain Recurrence, say.
4. Novel techniques. And of course, there is so much room for novel techniques for this novel problem!
