---
layout: post-custom
title: Beeping Busy Beaver
tag: busy-beaver
permalink: /2021/03/06/beeping-busy-beaver/
---

As a long time [Busy Beaver](https://en.wikipedia.org/wiki/Busy_beaver) enthusiast, I was very excited when I heard that Scott Aaronson published a [Busy Beaver survey](https://www.scottaaronson.com/blog/?p=4916) last September.<sup id="t1">[1](#f1)</sup> There are many exciting ideas and conjectures within it, but the one that has stuck with me the most was his proposed "Beeping Busy Beaver" function BBB(n). Quoting from the paper:

> One can define a function BB<sub>1</sub>(n), involving Turing machines with oracles for the original BB function, which grows uncomputably quickly even given an oracle for BB. Could we compute the first values of BB<sub>1</sub>? Alas, this is liable to be uninteresting, just because the details of how a Turing machine queries a BB oracle (by writing n onto an oracle tape, etc.) will involve many kludgy and non-canonical choices, and one might need many states before one saw the effect of those choices.
>
> But there’s an alternative. Define a beeping Turing machine to be exactly the same as one of the Turing machines except that it emits a "beep" as it exits a certain designated state, called the "beep state". Then given a beeping Turing machine M, let b(M) be the number of the last step on which M beeps (or b(M) := ∞ if M beeps infinitely often), when M is run on an initially all-0 input tape. Finally, define the n th Beeping Busy Beaver number "BBB(n)" to be the maximum (non-infinite) value of b(M) for all n-state Turing Machines M.
>
> Clearly BBB(n) ≥ BB(n) for all n, since we could designate the state from which M halts as its beep state. One can show that, as n gets larger, BBB must grow uncomputably faster than even BB—indeed, it grows at a similar rate to BB<sub>1</sub>, in the sense that BBB and BB<sub>1</sub> are both computable given an oracle for any upper bound on the other.

Nick Drozd [introduced the term "quasihalt"](https://nickdrozd.github.io/2020/08/13/beeping-busy-beavers.html) to denote the moment that a Beeping TM beeps last (TMs which beep infinitely often never quasihalt). This naming is very convenient and evocative because it makes the analogy between the Busy Beaver and Beeping Busy Beaver crystal clear: The Busy Beaver problem is to find the TM which runs longest before halting, the Beeping Busy Beaver problem is to find the TM which runs longest before quasihalting.

I was quite shocked to discover that such a simple modification to the Turing Machine halt condition was enough to give it the strength of an oracle machine! And at first it is not clear exactly why or how it accomplishes this. In the rest of this post I explore various aspects of the power of beeping TMs.


## Detecting Quasihalting

The first thing to note is that quasihalting is quite a bit more complicated that halting. While the question of whether a TM **will** halt is undecidable, the question of whether it **has** halted is extremely easy: is it in the Halt state? Deciding whether a TM has quasihalted, on the other hand is no simple task. It amounts to proving that the TM will never again reach a certain state. In fact, detecting whether a TM has quasihalted yet is an undecidable problem itself. We can see that by noting that if we had a reliable method for detecting that we had quasihalted, we could use it to solve the classic halting problem! Specifically, consider any arbitrary TM X, say:<sup id="t2">[2](#f2)</sup>

  |     |  0  |  1  |
  | :-: | :-: | :-: |
  |  A  | 1RB | 1LA |
  |  B  | 0RC | 1LC |
  |  C  | 1RD | 0LA |
  |  D  | 1RE | 1RB |
  |  E  | 1LC | 1R**H** |

We would like to know if it ever reaches the halt state **H**. Well, let's create a second TM Y by replacing the halt state with a beeping state "Q", make that the new start state and have it simply transition to the previous start state "A":

  |     |  0  |  1  |
  | :-: | :-: | :-: |
  | **Q** | **0RA** | --- |
  |  A  | 1RB | 1LA |
  |  B  | 0RC | 1LC |
  |  C  | 1RD | 0LA |
  |  D  | 1RE | 1RB |
  |  E  | 1LC | 1R**Q** |

So this new TM starts in state Q, then immediately transitions to the original TM (X)'s start state "A" and starts running on a blank tape. It will then act exactly the same as the original TM (X) until/unless it reaches the `E1 -> 1RQ` transition.

Has this new TM (Y) already quasihalted after it's first step? If it has, then that means the original TM (X) will never halt. If Y has not already quasihalted, that means that X will halt. So if we could decide whether an arbitrary TM has already quasihalted, we could decide the classical halting problem!


## The Power of Quasicomputing

OK, so clearly quasihalting is more _complicated_ than halting, but is it more _powerful_? What does it even mean to be more powerful? Well, for normal TMs, we say that they compute a function if, for any input `n` they can write `f(n)` on the tape and then halt. Any function `f` which can be computed by a TM is said to be **computable**. Famously, the Busy Beaver function is not computable and, in fact, it grows faster than any computable function. Reasoning about tape contents is a bit annoying and runtime is often more useful. So consider a TM that which given input `n` runs for `g(n)` steps and then halts. It can be shown that this `g` is also a computable function.

So then, let us say that a function `h` is "quasicomputable" if there exists a TM which when given input `n` runs for `h(n)` steps and then quasihalts. Is quasicomputablity stronger than computability? Yes! In fact, the classic Busy Beaver function is quasicomputable! (or to be a bit more technical, it is dominated by a quasicomputable function.)

Here is the algorithm for quasicomputing the Busy Beaver function:
* Given an input `n`, enumerate all `n`-state 2-symbol Turing machines onto subsections of the tape.
* In parallel, simulate each of these TMs (Simulate step 1 of TM 1, then step 1 of TM 2, ..., step 2 of TM 1, ...).
* Whenever any of the simulated TMs halts, enter the beeping state and continue with the other TMs.

So, this program will beep exactly once for every `n`-state TM that halts. And, as we know, there are only a finite number of `n`-state halting TMs. Therefore, this program will only beep a finite number of times and thus it quasihalts! But how many steps does it run before halting? Well to know the exact number would require me to give a much more detailed description, but for sure it will run for more steps than every single one of the halting `n`-state TMs runs for (because it is simulating every one of those steps before quasihalting). So this program computes a function `h(n) > BB(n)`. And thus we can see that quasicomputablity is stronger than computability (because `BB(n)` grows faster than any computable function)! Perhaps we should have called it "hyper-computability".


## Future work

This demonstration that the Busy Beaver function is quasicomputable seems very powerful, but we have still not completely shown that BBB(n) grows at the same rate as BB<sub>1</sub>(n) (the oracle-Busy Beaver). Likewise, Scott mentions in a footnote that we could have defined a similar function for Nondeterministic TMs and that that function would also grow at the same rate. In a future post I plan to dig into those comparisons.


## Footnotes

* <b id="f1">[1](#t1)</b> A small aside about Scott Aaronson: I first learned of Scott over a decade ago when I read his excellent 1998 essay [Who Can Name the Biggest Number?](https://www.scottaaronson.com/writings/bignumbers.html) which really captures the excitement I feel about the Busy Beaver problem so beautifully. I heard of him again in 2016 when his master's student Adam Yedidia constructed an explicit upper bound to the Busy Beaver number that ZFC can prove ([The 8000th Busy Beaver number eludes ZF set theory](https://www.scottaaronson.com/blog/?p=2725)) which was quite exciting.
* <b id="f2">[2](#t2)</b> This is one of the 8031 holdout 5-state, 2-symbol TMs that we believe never halts, but have been unable to prove that it runs infinitely. If only we could use a quasihalt detection algorithm on these, we could declare once and for all that BB(5) = 47,176,870.
