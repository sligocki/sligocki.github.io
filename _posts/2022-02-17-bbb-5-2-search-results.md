---
layout: post-custom
title: BBB(5, 2) Search Results
tag: busy-beaver
---

Searching for [Beeping Busy Beavers]({% post_url 2021-03-06-beeping-busy-beaver %}) is a harrowing task, since even detecting that a TM has beeped for the last time (quasihalted) is in general uncomputable. However, as Nick Drozd [has shown](https://nickdrozd.github.io/2022/02/11/latest-beeping-busy-beaver-results.html) over the last year or so, there is still progress to be made just looking at the types of quasihalting we can definitively prove. Over the last couple weeks, I've retooled [the codebase](https://github.com/sligocki/busy-beaver) (that my Dad and I have developed over the last 15 years) to be able to detect quasihalting behavior and then enumerated various domains.

This search has been very fruitful in the 5-state, 2-symbol search I would like to announce several new machines of interest:

## A new BBB(5, 2) champion:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LC |
|  B  | 1RC | 0RD |
|  C  | 0LB | 0RC |
|  D  | 0RE | 1RD |
|  E  | 1LE | 1LA |

`1RB1LC_1RC0RD_0LB0RC_0RE1RD_1LE1LA`

which I calculate as quasihalting on step:

<samp>
173435554530734876325688319985246476114761227213875751098091105080403058813
137700864058572534669440325871164552850233406790267431534345682690909996466
739822790829561588574981765612968902718598248994133711641212367730835044877
508837803451797867016673613501402694914688652744459261232089469884419360736
544397066449067999804481124350082304474584114473419538028647524012631393555
498144472772542970640769342167886331694446569461523622690305119525401331902
62122812094646680539368211780404865521194338123470785
</samp> (>1.7 × 10<sup>502</sup>)

After which point it is in state E on a blank tape and will now move to the left writing 1s forever (because `E0 -> 1LE`). It was last in state D one step earlier.

Nick, you might be curious to know that this machine is also a [Blanking Beaver](https://nickdrozd.github.io/2021/02/14/blanking-beavers.html). In fact, it actually reaches a blank tape in every one of it's 5 states! All 4 non-trivial one are near the end:

<samp>
22592  0<sup>∞</sup> C> 1<sup>~10<sup>251.4</sup></sup> 0<sup>∞</sup><br/>
22593  <b>0<sup>∞</sup> C> 0<sup>∞</sup></b><br/>
22594  <b>0<sup>∞</sup> <B 0<sup>∞</sup></b><br/>
22595  0<sup>∞</sup> 1<sup>1</sup> C> 0<sup>∞</sup><br/>
22596  0<sup>∞</sup> 1<sup>1</sup> <B 0<sup>∞</sup><br/>
22597  <b>0<sup>∞</sup> D> 0<sup>∞</sup></b><br/>
22598  <b>0<sup>∞</sup> E> 0<sup>∞</sup></b><br/>
</samp>

## A close runner up:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1LE |
|  B  | 0RC | 0RC |
|  C  | 0RD | 1RC |
|  D  | 1LD | 1LA |
|  E  | 0LB | 0RE |

`1RB1LE_0RC0RC_0RD1RC_1LD1LA_0LB0RE`

which quasihalts on step:

<samp>
173435554530734876325688319985246476114761227213875751098091105080403058813
137700864058572534669440325871164552850233406790267431534345682690909996466
739822790829561588574981765612968902718598248994133711641212367730835044877
50883780345179786701667361<u>1145572109</u>965185955923949932905433536564448131797
454831082435818948428216902541362169354919782004726583684978216073073971799
032636113747662512733692622512739028763729615094519436023437779549237498239
23282203790915605756125236274372232928765435094504615
</samp>

actually, when I first found these two machines, I mistakenly thought that this was the champion (partially because GNU `sort` silently fails to correctly numerically sort integers of this size!) I have underlined the section of digits in which these two numbers first differ (about halfway through the digit string).

I am working on an analysis of these two machines that I will share soon, but it appears that they are both simulating the exact same [Collatz-like]({% post_url 2021-07-17-bb-collatz %}) function. The champion just does it slightly more inefficiently ... and thus wins the race because in the Beeping Busy Beaver game we are actually looking for the slowest machine to cross the finish line :)

## A machine with (slightly) more complex quasihalting behavior

Almost all of the top machines I found (19 of the top 20, for example) quasihalt in an extremely easy to detect way. They enter into what I am calling "Chain Recurrence" (Nick calls this 1-step [Lin Recurrence](https://nickdrozd.github.io/2021/02/24/lin-recurrence-and-lins-algorithm.html)). It is the behavior I described above for the 10<sup>502</sup> champions I listed above. Specifically: we are in configuration <code>S> 0<sup>∞</sup></code> and `S0 -> ?RS` (or the mirror image: <code>0<sup>∞</sup> <S</code> and `S0 -> ?LS`) and so we can easily see that the TM will remain in state S forever after, just moving right (or left) over and endless stream of 0s.

Here is the 1 top 20 machine I found so far which does something slightly more complex:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1RD |
|  B  | 1LC | 1LB |
|  C  | 1LD | 1RA |
|  D  | 0RE | 0RD |
|  E  | 1LB | 1RE |

`1RB1RD_1LC1LB_1LD1RA_0RE0RD_1LB1RE`

It quasihalts at step 94418964659944351442398224758 (>9.4 × 10<sup>28</sup>), blanks the tape at step 12250514892052349453616935044134 (>1.2 × 10<sup>31</sup>) and then enters into what Brady called "Christmas Tree" Recurrence. Explicitly:

for any `n ≥ 0`:

<samp>
  0<sup>∞</sup> <D 1<sup>n</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> E> 1<sup>n</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> 1<sup>n</sup> E> 0<sup>∞</sup><br/>
  0<sup>∞</sup> 1<sup>n</sup> <B 1 0<sup>∞</sup><br/>
  0<sup>∞</sup> <B 1<sup>n+1</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> <C 1<sup>n+2</sup> 0<sup>∞</sup><br/>
  0<sup>∞</sup> <D 1<sup>n+3</sup> 0<sup>∞</sup><br/>
</samp>

and then this process repeats forever, increasing the number of `1`s by 3 each recurrence. Note that this recurrence never enters state `A`, which is how we detect that this machine actually quasihalted much earlier.

## Enumeration Status

I thought that some of you might be interested in the overall size of the BBB landscape. I've enumerated TMs in the following domains:

| Domain | max # loops |       # TMs | # Quasihalt | % Quasihalt |  # Unknown | % Unknown |
|:------:|:-----------:|------------:|------------:|------------:|-----------:|----------:|
|   2x2  | 1M          |         121 |          17 |       14.0% |          1 |      0.8% |
|   2x3  | 1M          |      10,393 |         974 |        9.4% |        192 |      1.8% |
|   3x2  | 1M          |      16,549 |       4,249 |       25.7% |        112 |      0.7% |
|   2x4  | 1M          |   1,869,525 |     133,612 |        7.1% |     69,331 |      3.7% |
|   4x2  | 1M          |   2,943,669 |     919,061 |       31.2% |     26,136 |      0.9% |
|   3x3  | 1k          | 164,537,029 |  26,451,658 |       16.1% |  5,086,812 |      3.1% |
|   2x5  | 1k          | 548,090,457 |  32,489,678 |        5.9% | 44,087,899 |      8.0% |
|   5x2  | 50k         | 687,161,765 | 248,328,604 |       36.1% | 10,822,343 |      1.6% |

* Domain: NxM means N-state, M-symbol
* \# TMs: the number of TMs enumerated using Tree Normal Form (Nick calls this Brady's algorithm) where the first transition is always forced to be `A0 -> 1RB` and we allow machines with no halt transitions.
* max \# loops: Number of "simulator loops" I have run all of the TMs in this domain for. Note that this is an accelerated simulator, so one sim loop can translate into a large number of TM steps. \# loops is roughly proportional to simulation time.
* \# Quasihalt: does not include \# Halt
* \# Unknown: The number of "holdout" machines which our system has not been able to make any conclusion about, if you can get this to 0, you've "proven" the BBB for that domain.

Everything up to 4x2 case can be run serially locally (it took me 7hr to run 4x2 out to 10k loops on my laptop). For the 3 biggest domains (3x3, 2x5, 5x2) I have been running in parallel on a cluster of 20 12-core machines. Running 5x2 out to 50k took about 4300 core-hours (~18 hours on the cluster). I ran 5x2 up to 1k, 2k, 10k, 25k, and 50k and up until 25k each increase led to a new BBB(5, 2) champion. 50k run did not find anything new so I decided it was a good time to share the news :)

All the code to enumerate and simulate these domains is available at our [Busy Beaver repo](https://github.com/sligocki/busy-beaver). And some results are also available there. For example, my top 20 BBB(5, 2) champions are at: <https://github.com/sligocki/busy-beaver/blob/main/Machines/5x2-Beep-Top>. The code is perhaps not the most user-friendly / documented at the moment, but I'm currently working on improving that. [Send me an email](mailto:sligocki@gmail.com) if your interested in playing around with this or using it to analyze long running Beavers.

## Reflection

Even with the simplest of quasihalting conditions (Chain Recurrence), we've been able to push BBB(5,&nbsp;2) > 10<sup>502</sup>. Is there more out there? I imagine yes. While almost all of the TMs we proved quasihalting were proven using Chain/Lin Recurrence, there is a clear selection bias here. We only found what our system was programmed to find. Among the > 10M remaining Unknown TMs, I predict that there will be some monsters which exhibit far more complex quasihalting behavior ... but how do we find them? I excited look forward to new techniques being developed and deployed!
