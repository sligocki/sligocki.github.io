---
layout: post
title: Nondeterministic Busy Beaver
tag: busy-beaver
---

In my [last post]({% post_url 2021-03-06-beeping-busy-beaver %}), I started exploring the power of Beeping Turing Machines (a concept introduced in Scott Aaronson's recent [Busy Beaver survey](https://www.scottaaronson.com/blog/?p=4916)). In this post I will explore the connection between Nondeterministic Turing Machines (NTMs) and Beeping TMs. In a footnote of his Beeping Busy Beaver section, Scott Aaronson mentions:

> A different way to get BB<sub>1</sub>-like growth, which was actually [Harvey] Friedman’s original suggestion, would be to define a Busy Beaver function for nondeterministic Turing machines, with the "running time" of such a machine taken to be the number of steps in its longest halting path. However, BBB struck me as more amenable to empirical study for small values of n.

As with the connection between Beeping TMs and Oracle Machines, this is not at all obvious to me. However, I'm more familiar with NTMs than Beeping TMs (having learned about them in my Undergraduate Decidability and Tractability course) so I thought that could be an interesting connection to explore.


## Definitions

Perhaps a good place to start would be to define a few things clearly. For a normal (deterministic) Turing Machine, the transition table maps every input: `(internal state, symbol read from tape)` to a single specific transition: `(symbol to write to tape, direction to move, new state)`. For a Nondeterministic Turing Machine (NTM), we allow each input to be mapped to **any** number of transitions. There are two interpretations of NTMs: (1) That every time it steps, it chooses one of these transitions, but we do not know which one it will choose or (2) It chooses all the transitions and thus it's computation is not a linear sequence of configurations, but a branching graph all possible configurations that could be reached from a given starting point.

Let us re-use names and say that an NTM "quasihalts" ...


## Simulating a Beeping TM with an NTM

As it turns out, for any Beeping TM, there is a simple modification to transform it into an NTM which quasihalts at the same step number as the original Beeping TM:

For every transition in the Beeping TM: `(state, symbol_in) -> (symbol_out, dir, beep_state)` add a new transition: `(state, symbol_in) -> (symbol_out, dir, Halt)`. So for example, consider the [current reigning 4-state, 2-symbol BBB champion](https://nickdrozd.github.io/2020/10/09/beeping-busy-beaver-results.html#orgc0f5dc1) is:

  |     |  0  |  1  |
  | :-: | :-: | :-: |
  |  A  | 1RB | 0LC |
  |  B  | 1LD | 0LA |
  |  C  | 1RC | 1RD |
  |  D  | 1LA | 0LD |

with Beeping state "A". So then let the NTM be defined as:

  |     |  0  |  1  |
  | :-: | :-: | :-: |
  |  A  | 1RB | 0LC |
  |  B  | 1LD | 0LA<br>0L**H** |
  |  C  | 1RC | 1RD |
  |  D  | 1LA<br>1L**H** | 0LD |

What possible paths can this NTM take? Well, it actually has very few options, it will actually do the exact same thing as the original Beeping TM does until we get to one of the two transitions which originally went to state "A". At that point, the NTM will have two options either halt, or continue following the Beeping TMs path. Well then, we can see that the steps at which the NTM could halt are exactly the same as the steps when the Beeping TM beeped and so they both quasihalt at the exact same step!

This elegantly proves that `NBB(n) ≥ BBB(n)`.


## Simulating an NTM with a Beeping TM
