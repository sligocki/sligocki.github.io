---
layout: post
title: Beeping Busy Beaver
---

As a long time [https://en.wikipedia.org/wiki/Busy_beaver](Busy Beaver) aficionado, I was very excited when I heard that Scott Aaronson published a [https://www.scottaaronson.com/blog/?p=4916](Busy Beaver survey) last September. There are many exciting ideas and conjectures within it, but the one that has stuck with me the most was his proposed "Beeping Busy Beaver" function BBB(n). Quoting from the paper:

> One can define a function BB<sub>1</sub>(n), involving Turing machines with oracles for the original BB function, which grows uncomputably quickly even given an oracle for BB. Could we compute the first values of BB<sub>1</sub>? Alas, this is liable to be uninteresting, just because the details of how a Turing machine queries a BB oracle (by writing n onto an oracle tape, etc.) will involve many kludgy and non-canonical choices, and one might need many states before one saw the effect of those choices.
>
> But there’s an alternative. Define a beeping Turing machine to be exactly the same as one of the Turing machines except that it emits a "beep" as it exits a certain designated state, called the "beep state". Then given a beeping Turing machine M, let b(M) be the number of the last step on which M beeps (or b(M) := ∞ if M beeps infinitely often), when M is run on an initially all-0 input tape. Finally, define the n th Beeping Busy Beaver number "BBB(n)" to be the maximum (non-infinite) value of b(M) for all n-state Turing Machines M.
>
> Clearly BBB(n) ≥ BB(n) for all n, since we could designate the state from which M halts as its beep state. One can show that, as n gets larger, BBB must grow uncomputably faster than even BB—indeed, it grows at a similar rate to BB<sub>1</sub>, in the sense that BBB and BB<sub>1</sub> are both computable given an oracle for any upper bound on the other.

Nick Drozd [https://nickdrozd.github.io/2020/08/13/beeping-busy-beavers.html](introduced the term "quasihalt") to denote the moment that a Beeping TM beeps last (TMs which beep infinitely often never quasihalt). This naming is very convenient and evocative because it makes the analogy between the Busy Beaver and Beeping Busy Beaver crystal clear: The Busy Beaver problem is to find the TM which runs longest before halting, the Beeping Busy Beaver problem is to find the TM which runs longest before quasihalting.

I was quite shocked to discover that such a simple modification to the Turing Machine halt condition was enough to give it the strength of an oracle machine! And at first it is not clear exactly why or how it accomplishes this. In the rest of this post I explore various aspects of the power of beeping TMs.


## Detecting Quasihalting

The first thing to note is that quasihalting is quite a bit more complicated that halting. While the question of whether a TM **will** halt is undecidable, the question of whether it **has** halted is extremely easy: is it in the Halt state? Deciding whether a TM has quasihalted, on the other hand is no simple task. It amounts to proving that the TM will never again reach a certain state. In fact, detecting whether a TM has quasihalted yet is an undecidable problem itself. We can see that by noting that if we had a reliable method for detecting that we had quasihalted, we could use it to solve the halting problem! Specifically, consider any arbitrary TM X, say:

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

So this new TM starts in state Q, then immediately transitions to the original TM (X)'s start state "A" and starts running on a blank tape. It will then act exactly the same as the original TM (X) until/unless it reaches the E1 -> 1RQ transition.

Has this new TM (Y) already quasihalted after it's first step? If it has, then that means the original TM (X) will never halt. If Y has not already quasihalted, that means that X will halt. So if we could decide whether a TM has already quasihalted, we could decide the halting problem!


## The power of Quasicomputing




A small aside about Scott: I first learned of Scott over a decade ago when I read his excellent 1998 essay [https://www.scottaaronson.com/writings/bignumbers.html](Who Can Name the Biggest Number?) which really captures the excitement I feel about the Busy Beaver problem so beautifully. I heard of him again in 2016 when his master's student Adam Yedidia constructed an explicit upper bound to the Busy Beaver number that ZFC can prove ([https://www.scottaaronson.com/blog/?p=2725](The 8000th Busy Beaver number eludes ZF set theory)).
