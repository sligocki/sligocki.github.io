---
layout: post-custom
title: Eclipse Machines
tags: busy-beaver cryptid
---

[Lucy's Moonlight]({% post_url 2025-04-21-lucys-moonlight %}) is not the only BB(6) Cryptid that has a non-negligible chance of beating the current [10↑↑15 champion]({% post_url 2022-06-21-bb-6-2-t15 %}). In fact there are 6 others found since July 2024 (including one family of 16 with the same behavior). It would be useful to have a name to refer to these types of machines by. Racheline suggested the name **Eclipse machines**.


## Definition

An Eclipse machine is a [probviously](https://wiki.bbchallenge.org/wiki/Probvious) halting [Cryptid](https://wiki.bbchallenge.org/wiki/Cryptids) which has some chance (but not guaranteed) to beat the current champion. 

TODO: In fact, we will find it helpful to extend this definition slightly to say that 

In fact, it was not even the first to be found: that appears to be `1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA1LD_1RZ0LC` discovered by `@mxdys` on 30 Jun 2024. However, 

## Known Eclipse Machines

The following list was compiled by Racheline on 2 Mar 2025 ([Discord link](https://discord.com/channels/960643023006490684/1239205785913790465/1345751155325665340)). I have not personally verified most of these results, but wanted to share them with a wider audience.

|  ID  | Champion Chance | Champion Chance |  TM                                      |  Links   |
| :--: | :------ | :------------------: | :-----------------------------------------: | :------- |
| `e1` | 5.50%   | $$\frac{4}{5}^{13}$$ | `1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_1RZ0RA` | [Lucy's Moonlight]({% post_url 2025-04-21-lucys-moonlight %}) |
| `e2` | 3.16%   | $$\frac{3}{4}^{12}$$ | `1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_1RZ0LE` | [wiki](https://wiki.bbchallenge.org/wiki/1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_---0LE) |
| `e3` | 0.77%   | $$\frac{2}{3}^{12}$$ | `1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA1LD_1RZ0LC` | [wiki](https://wiki.bbchallenge.org/wiki/1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC) |
| `e4` | 0.51%   | $$\frac{2}{3}^{13}$$ | `1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_1RZ1LB` | [wiki](https://wiki.bbchallenge.org/wiki/1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB) |
| `e5` | 0.012%  | $$\frac{1}{2}^{13}$$ | `1RB0LC_0LC0RF_1RD1LC_0RA1LE_1RZ0LD_1LF1LA` | [wiki](https://wiki.bbchallenge.org/wiki/1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA) |
| `e6` | 0.012%  | $$\frac{1}{2}^{13}$$ | `1RB1RE_1LC1LD_1RZ1LA_1LB1LE_0RF0RA_1LD1RF` | [wiki](https://wiki.bbchallenge.org/wiki/1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF) |
| `e7` | 0.0049% | $$\frac{2}{5} \cdot \frac{1}{2}^{13}$$ | `1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_1RZ1RE` | [wiki](https://wiki.bbchallenge.org/wiki/1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE) |

A few notes:

* `e2`, `e3`, `e4`, `e6` and `e7` all iterate the [Hydra function](https://wiki.bbchallenge.org/wiki/Hydra_function) in there version of "Main phase".
* `e3` represents a collection of 16 different TMs (in [TNF](https://wiki.bbchallenge.org/wiki/Tree_Normal_Form)) which all share the same behavior. See the [wiki page](https://wiki.bbchallenge.org/wiki/1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC) for more details.
* `e3` was first discovered by `@mxdys`. The rest were all found by Racheline.
* `e7` probviously either halts or becomes a [Translated Cycler](https://wiki.bbchallenge.org/wiki/Translated_cycler). It has a 40% chance to eventually halt and a 60% chance to eventually become a Translated Cycler. It is the only TM on this list which does not have a 100% of probviously halting.

...

## Origins

Many of these TMs seem to have been found when @mxdys ran Pavel's decider across a range of BB(6) holdouts and 28 caused that program to OOM ([Discord Link](https://discord.com/channels/960643023006490684/1239205785913790465/1266428071817379862) from 26 Jul 2024).


Unclear result:

1RB1RA_1LC1LE_0LD0LB_0RA0RF_0LC0RA_1RD---
  https://discord.com/channels/960643023006490684/1267928440506421283/1267928443173998665

Analyzed:

https://wiki.bbchallenge.org/wiki/Antihydra
  1RB1RA_0LC1LE_1LD1LC_1LA0LB_1LF1RE_---0RA antihydra
https://wiki.bbchallenge.org/wiki/1RB1RC_1LC1LE_1RA1RD_0RF0RE_1LA0LB_---1RA
  1RB1RC_1LC1LE_1RA1RD_0RF0RE_1LA0LB_---1RA antihydra-like?
https://wiki.bbchallenge.org/wiki/1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC
  1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA1LD_---0LC halting cryptid candidate
  1RB1RA_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC "halting cryptid candidate"-like
  1RB0RD_0RC1RC_1LD0LF_0LE1LE_1RA1LD_---0LC "halting cryptid candidate"-like
  1RB1RA_0RC1RF_1LD---_0LE1LE_1RA0LB_1LD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RF_1RD---_0LE1LE_1RA1LD_1LD0LF "halting cryptid candidate"-like
  1RB1RA_0RC1RF_1RD---_0LE1LE_1RA1LD_1LD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RF_1LD---_0LE1LE_1RA1LD_1RD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RC_1LD0LF_0LE1LE_1RA0LB_---0LC "halting cryptid candidate"-like
  1RB1RA_0RC1RF_1RD---_0LE1LE_1RA1LD_1RD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RF_1LD---_0LE1LE_1RA1LD_1LD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RC_1RD0LF_0LE1LE_1RA1LD_---0LC "halting cryptid candidate"-like
  1RB0RD_0RC1RF_1LD---_0LE1LE_1RA0LB_1LD0LF "halting cryptid candidate"-like
  1RB0RD_0RC1RF_1RD---_0LE1LE_1RA1LD_1RD0LF "halting cryptid candidate"-like
  1RB1RA_0RC1RF_1LD---_0LE1LE_1RA1LD_1LD0LF "halting cryptid candidate"-like
  1RB1RA_0RC1RF_1LD---_0LE1LE_1RA1LD_1RD0LF "halting cryptid candidate"-like
  1RB1RA_0RC1RC_1RD0LF_0LE1LE_1RA1LD_---0LC "halting cryptid candidate"-like
https://wiki.bbchallenge.org/wiki/1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF
  1RB1RE_1LC1LD_---1LA_1LB1LE_0RF0RA_1LD1RF 
https://wiki.bbchallenge.org/wiki/1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB
  1RB0RE_1LC1LD_0RA0LD_1LB0LA_1RF1RA_---1LB "halting cryptid candidate"-like? -2,-1
https://wiki.bbchallenge.org/wiki/1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA
  1RB0LC_0LC0RF_1RD1LC_0RA1LE_---0LD_1LF1LA "halting cryptid candidate"-like? -3,-1
https://wiki.bbchallenge.org/wiki/1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE
  1RB1LE_0LC0LB_1RD1LC_1RD1RA_1RF0LA_---1RE "halting cryptid candidate"-like? -3,-2

Not Cryptids:

Halting: 
  https://wiki.bbchallenge.org/wiki/1RB0RC_0LC0LB_0LD1LC_0LE1LA_0LF---_1RF1RA
    1RB0RC_0LC0LB_0LD1LC_0LE1LA_0LF---_1RF1RA "halting cryptid candidate"-like? -3,-2
Non-halting:
  https://wiki.bbchallenge.org/wiki/1RB0LE_1LC1RA_---1LD_0RB1LF_1RD1LA_0LA0RD
    1RB0LE_1LC1RA_---1LD_0RB1LF_1RD1LA_0LA0RD
    1RB0LE_1LC1RA_---1LD_0RB1LF_1RD1LA_1RD0LA
    1RB0LE_1LC1RA_---1LD_0RB1LF_1RD1LA_0LA0LA
  https://discord.com/channels/960643023006490684/1239205785913790465/1267578777370755146
    1RB0RF_1LC0RA_0LD0LB_0RE1LF_1LA---_0RC0LE


== Other ==

https://wiki.bbchallenge.org/wiki/1RB0LC_1LC0RD_1LF1LA_1LB1RE_1RB1LE_---0LE
https://wiki.bbchallenge.org/wiki/1RB0RD_0RC1RE_1RD0LA_1LE1LC_1RF0LD_---0RA