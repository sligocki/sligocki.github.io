---
layout: post
title: Filter - Static Back
tags: busy-beaver
---

Over the decades since the Busy Beaver game was first proposed, the process of proving values has come to solidify into the following:

 1. Enumerate all TMs (perhaps up to equivalence using, say, [Brady's TNF algorithm](https://nickdrozd.github.io/2022/01/14/bradys-algorithm.html))
 2. Run all halting TMs until they halt
 3. Run a sequence of "filters" on all non-halting TMs to incrementally prove that they will never halt.
 4. Analyze a small number of remaining "holdout" machines by hand to prove they will never halt.

This is the process that Radó used to prove `BB(2, 2)` (using no filters), Lin used to prove `B(3, 2)` (using a single filter: [Lin Recurrence](https://nickdrozd.github.io/2021/02/24/lin-recurrence-and-lins-algorithm.html) / [Translation Cycling](https://discuss.bbchallenge.org/t/decider-translated-cyclers/34)) and Brady used to prove BB(4, 2) (with several filters for various tree/counter detections). Likewise, it is the model that the folks at bbchallenge.org are using to try and prove BB(5, 2).

Each step in this process has its own challenges, but step (3) (creating filters to prove non-halting) is (I believe) the biggest challenge to proving BB(5, 2). In order to support this mission, I will be sharing details about various filters that my father and I use in [our codebase](https://github.com/sligocki/busy-beaver).

Note that what I call "filters" the bbchallenge.org folks call "deciders".

## Filter: Static Back

The first filter I will share is the *Static Back* filter (In our codebase this is called the Reverse Engineering filter ... but this is a terrible name, so let's forget it). This is sort of a type of "Backwards Reasoning" filter (like Backtracking) that attempts to prove that a TM can never reach the halt state not by looking at how it acts, but by looking at how it would have to act in order to halt and discovered that that is impossible. But unlike *Backtracking*, this filter is static. It does not simulate backwards instead it simply checks a few conditions on the transition table and thus it is simple to implement and fast to run.

The logic of *Static Back* is pretty straightforward.

For each transition to halt in your machine `(state s, symbol q) -> Halt` (Where symbol `q` is not the blank symbol):

 * Do all other transitions into state `s` move in only one direction? (Ex: Left)
 * Do all other transitions which write symbol `q` move in the same direction? (Ex: also Left)
 * If both of these are true, then we can never reach this `(state s, symbol q) -> Halt` transition because all of the symbols `q` will be to the right of the TM head, but we will only enter state `s` moving left.

(Note that this does not work if `q` is the blank symbol because there are always infinitely many blank symbols to both the left and right of the TM head at all times.)

## Prevalence

Does this situation really come up very often? It turns out that it is *reasonably* common. When enumerated in TNF:
 * ~20% (88k / 620k) of 4x2 TMs
 * ~28% (59k / 348k) of 2x4 TMs
are classified as non-halting via this filter. Of course, many other filters would have caught these TMs eventually ... but this filter is so fast, I always run it first!

As discussed in a [recent email](https://groups.google.com/g/busy-beaver-discuss/c/jH9Y6MZPisY/m/wff8oq3JBAAJ), this filter is more effective for the multi-symbol case. But even for the binary tape, I think it is worth running.

## Attribution

As far as I'm aware, I invented this technique around the summer of 2006. I had just read a paper discussing *Backtracking* and I did not understand it correctly and developed this *Static Back* technique instead. But if anyone is aware of a pre-existing documentation of this, please let me know and I will update this page!


<!-- ... Note: It looks like most of these are actually just machines with A1 -> Halt and no trans -> A ... -->
