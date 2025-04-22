---
layout: post
title: A Regex-Proof TM
tags: busy-beaver ctl
---

Over the last several months there has been a ton of exciting work done expanding on the [CTL filter]({% post_url 2022-06-10-ctl %})) I described here back in June. This has all been happening under the umbrella of the bbchallenge.org community. Things have been moving so quickly there I haven't even been able to catch up with it all. Justin Blanchard (@UncombedCoconut) wrote up a [nice summary](https://discuss.bbchallenge.org/t/closed-tape-languages-asymmetric-automata-construction/117) in the bbchallenge.org forum including:

* @Iijil: https://github.com/Iijil1/Bruteforce-CTL/
* @djmati1111: https://github.com/colette-b/bbchallenge (z3-powered search)
* @Frans_Faase: https://github.com/FransFaase/SymbolicTM (proof verifier / guided solver)
* Justin Blanchard (@UncombedCoconut): https://github.com/UncombedCoconut/bbchallenge/blob/main/decide_closed_tape_language_l2r.py

In [another of Justin's posts](https://discuss.bbchallenge.org/t/7410754-does-not-halt/100a), he formalizes the problem of defining a Closed Tape Language by introducing a new notation compact notation (let's call it "Coconut notation") for TM configurations that I found quite elegant and useful. Quoting from his post, Coconut notation is:

> We model a TM’s state TM as a word in the language `[01]*[a-eA-E][01]*`
> Lower-case a-e represent the TM head in state A-E on top of a 0 bit.
> Upper-case A-E represent the TM head in state A-E on top of a 1 bit.
> A “Word” is just a string (with designated left/state/right components).
> The TM [configurations] are Words modulo leading/trailing zeros.
> Words (NOT modulo leading/trailing zeros) can also represent TM-[config] infixes.

So, for example, the configuration $$0^\infty 1 C> 1^3 01 0^\infty$$ would be rendered as `1C1101` (or equivalently with any number of leading or trailing 0s, like `001C11010`) while the config $$0^\infty 1 B> 0^3 101 0^\infty$$ would be rendered as `1b00101`. Notable things about Coconut notation:

1. We ignore the $$0^\infty$$ at each end.
2. The current state and current symbol are combined into a single character using lower- and upper-case to distinguish between current symbol 0 or 1. (Note: Unfortunately, Coconut notation does not extend well to multi-symbol TMs).

Now that we have encoded TM configurations into simple strings we can define CTL generally as any Regular Language (Language recognized by a Regex) $$L$$ over these strings with the 3 properties:

1. The initial config (`a`) is in $$L$$
2. For any config in $$L$$, it's successor config (the config after taking one TM step) is also in $$L$$ ($$L$$ is closed under TM step).
3. $$L$$ does not contain any halting configurations.

This formalization allows for more powerful proofs than the formulation I described in my [CTL post example](https://www.sligocki.com/2022/06/10/ctl.html#example). In my example (and in the CTL filter in my codebase) the regular expressions for the left and right sides of the tape are independent, but with Coconut notation, you can make restrictions that cross the TM head. So for example, With this notation
