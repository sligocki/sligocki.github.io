---
layout: post
title: Standard TM Text Format
tags: busy-beaver
---

I will be using a new standard one-line TM text format for all of my TM examples moving forward, it looks like this:

```
1RB2LA1RA1RA_1LB1LA3RB1RZ
1RB0LD_1RC0RF_1LC1LA_0LE1RZ_1LF0RB_0RC0RE
1RB1RC_1RC1RB_1LD0RA_---1LE_0LD0LA
```

Which represents the 3 TMs:
 * My dad and my reigning 2-state, 4-symbol Busy Beaver champion (See [Pascal Michel's historical survey](https://webusers.imj-prg.fr/~pascal.michel/ha.html#tm24)).
 * Pavel's reigning 6-state, 2-symbol Busy Beaver champion (See [BB(6, 2) > 10↑↑15]({% post_url 2022-06-21-bb-6-2-t15 %})).
 * A 5-state, 2-symbol TM which prints Sierpinski triangles found by bbchallenge.org Discord user @lijil (See visualization at [bbchallenge](https://bbchallenge.org/1RB1RC_1RC1RB_1LD0RA_---1LE_0LD0LA)).

Notably, I called this a "standard" format above because it is not only the format that I will be using moving forward, but also that will be (and is already being) used by several other Busy Beaver hunters. Specifically, this format was chosen through a [month-long discussion](https://discuss.bbchallenge.org/t/standard-tm-text-format/60) in June on the bbchallenge.org forum along with:
 * [Tristan Stérin](https://dna.hamilton.ie/tsterin/) (@cosmo), the primary maintainer of bbchallenge.org.
 * Pavel Kropitz (@univerz) of BB(6) fame who has a lot of cutting edge BB simulation software.
 * [Nick Drozd](https://nickdrozd.github.io/), a prolific Beeping Busy Beaver blogger.
 * And a couple other bbchallenge.org users (@star and @mateon1)

It is integrated into bbchallenge.org and so now you can see the page for any TM I present by simply constructing a URL using the TM in this standard format. Like https://bbchallenge.org/1RB2LA1RA1RA_1LB1LA3RB1RZ . This is also the standard format being used by most people to share and compare TM filtering results on the (very active) bbchallenge [Discord server](https://discord.gg/XY9wMZr7HX). I highly recommend it for wide use in the community.


## Format Definition

This format is a one-line (compressed version) of traditional TM transition tables. Traditionally, transition tables are written like:

|     |  0  |  1  |  2  |
| :-: | :-: | :-: | :-: |
|  A  | 1RB | 2LA | 1RA |
|  B  | 2LC | --- | 2RA |
|  C  | 2LA | 1LC | 1RZ |

Where capital letters A-... represent states, A is the initial state, Z is the halt state[^Halt], digits 0-... represent symbols and 0 is the blank symbol and `---` represents an undefined transition.

[^Halt]: more commonly called H historically, but I prefer Z to allow 8+ state machines.

So, let's say you are in state "A" and read a "2", then looking that up in the table you see transition instruction "1LC" which means: Write a "1", move Left, and enter state "C".

To convert from this tabular format to the standard one-line format, first concatenate all instructions on each row together (to get strings `1RB2LA1RA`, `2LC---2RA` and `2LA1LC1RZ`) and then combine these in order from top to bottom adding a "_" (underscore) between each row: `1RB2LA1RA_2LC---2RA_2LA1LC1RZ`.


### Important Details

This format requires the following standards:
 * Symbols must be represented by digits starting from "0" (and sequentially up to the number of symbols) and the blank symbol must be labeled "0". (Since each symbol is only one digit, this format only works for TMs with 10 or fewer symbols).
 * States must be represented by capital letters starting from "A" (and sequentially up to the number of states) and the start state must be labeled "A". (Since each state is represented by a capital letter, the maximum number of non-halting states is 25).
 * According to our standard, the halt state may be labeled with any capital letter larger than the number of TM states. However, I highly encourage the use of either "Z" or "H" for readability (H for halt, or Z as the letter allowing the largest number of preceding states).
 * Directions must be specified by "L" or "R" (for Left or Right).
 * The order of an individual transition instruction must be (symbol, dir, state) as seen above.
 * Transitions may be undefined, in which case they are written as `---` (3 dashes).
 * The original table must be oriented so that states key rows and symbols key columns and they must be sorted without any gaps (you cannot use states A, B, D with no C).
 * No separator is needed when concatenating a row together because each transition instruction is exactly 3 characters long.
 * An "_" is required to separate rows because otherwise there would be ambiguity between say 4-state, 2-symbol TMs and 2-state, 4-symbol TMs.
