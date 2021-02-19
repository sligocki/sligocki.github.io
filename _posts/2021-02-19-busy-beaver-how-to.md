---
layout: post
title: How to Compute Busy Beavers
---

The [Busy Beaver problem](https://en.wikipedia.org/wiki/Busy_beaver) is the puzzle of finding the Turing Machine of a given class which runs for the longest time, but eventually halts. It is famously uncomputable in general, however in the 60 years since Tibor Radó introduced the problem, [4 exact values](https://oeis.org/A060843) have been proven. Although the precise techniques and technology used to find and prove Busy Beaver candidates over time has advanced quite a bit, as far as I can tell the general outline of all proofs and attempts at proof have remained the same and break down into 3 steps:

1. Enumerate all TMs of the given class
2. Simulate all halting TMs until they halt
3. Prove that all non-halting TMs will never halt

