---
layout: post-custom
title: Green's Machines
tags: busy-beaver
---

In 1964 (only 2 years after Tibor Radó first described the Busy Beaver game), Milton W. Green of the Stanford Research Institute hand-crafted a family of fast-growing Turing Machines with $$n$$ states, 2 symbols for all $$n \ge 4$$.[^green64] At the time of publication, Green's Machines were the Busy Beaver champions for $$BB(n)$$ for all $$n \ge 6$$. This family grows roughly as fast as the [Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function)

$$BB_{2n} \approx 3 \uparrow^{n-2} 3$$

and (as far as I can tell) some of them have not been beaten to this day.

Green's Machines are still referenced often in discussion about Busy Beavers. However, I have not seen much discussion about the machines themselves. Even reading the original paper is a bit of a challenge. In it Green defines 4 different families of Turing Machines ($$M_N, G_N, B_N, BB_N$$) and I find the subtleties of how each is defined and extended to the other families to be quite confusing every time I read it. In order to support more widespread understanding of the details of Green's constructions, this post will describe Green's Machines and their scores in modern notation.

> Note: I use names in this article that are adapted from Green's names, but are not quite identical. The difference is somewhat subtle.


## The Machines

The Green Machines are made up of two components, split up by states. These components will be a setup component $$S_k$$ and a compute component $$G_n$$.


### G Component

The main workhorse is the $$G_n$$ component. For every odd $$n$$, $$G_n$$ defined by the transition table:

|  State      |  0              |  1              |
| :---------: | :-------------: | :-------------: |
| $$Q_n$$     | $$1 L T_n$$     | $$1 R Q_{n+2}$$ |
| $$T_n$$     | $$0 L Q_{n-2}$$ | $$0 L T_n$$     |
|   ...       |   ...           |   ...           |
| $$Q_k$$     | $$1 L T_k$$     | $$1 R Q_{k+2}$$ |
| $$T_k$$     | $$0 L Q_{k-2}$$ | $$0 L T_k$$     |
|   ...       |   ...           |   ...           |
| $$Q_1$$     | $$1 R Q_1$$     | $$1 R Q_3$$     |

Where the $$Q_k, T_k$$ are defined this way for all odd $$3 \le k \le n$$.

This component has a single entry state ($$Q_n$$) which the setup component should transition into when it wants to run the $$G_n$$ component and a single exit state ($$Q_{n+2}$$) which the component will transition out to when it is done with it's computation.

These compute a series of fast-growing functions $$G_n(k)$$ that I will describe in detail below.


### Setup Components

There are two different setup components $$S_3$$ and $$S_4$$ (with 3 and 4 states respectively). Either can be added to any $$G_n$$ component in order to make a Green Machine of either even or odd number of states (respectively).


#### Three-State Setup

The $$S_3$$ component is defined by:

|  State            |  0          |  1      |
| :---------------: | :---------: | :-----: |
|  $$A = Q_{n+2}$$  | $$1LB$$     | $$1RZ$$ |
|  $$B$$            | $$0LT$$     | $$1LT$$ |
|  $$T$$            | $$0 L Q_n$$ | $$0LT$$ |

where $$Q_n$$ is the entry state and $$A = Q_{n+2}$$ is the exit state of the $$G_n$$ component.

When attached to $$G_{2k+1}$$, this produces an even state $$BB_{2k+4}$$ Green Machine (with start state `A` and halt state `Z`).


#### Four-State Setup

The $$S_4$$ component is defined by:

|  State            |  0          |  1      |
| :---------------: | :---------: | :-----: |
|  $$A$$            | $$1LT$$     | $$1LB$$ |
|  $$B$$            | $$1LZ$$     | $$1LA$$ |
|  $$C = Q_{n+2}$$  | $$0LB$$     | $$1LT$$ |
|  $$T$$            | $$0 L Q_n$$ | $$0LT$$ |

where $$Q_n$$ is the entry state and $$C = Q_{n+2}$$ is the exit state of the $$G_n$$ component.

which, when attached to $$G_{2k+1}$$, this produces an odd state $$BB_{2k+5}$$ Green Machine (with start state `A` and halt state `Z`).


## Example Machines

Putting this all together and converting into [Standard Text format]({% post_url 2022-10-09-standard-tm-format %}) we get for example:

* $$BB_7$$: `1LD1LB_1LZ1LA_0LB1LD_0LE0LD_1LF1RC_0LG0LF_1RG1RE` ([on bbchallenge](https://bbchallenge.org/1LD1LB_1LZ1LA_0LB1LD_0LE0LD_1LF1RC_0LG0LF_1RG1RE))
* $$BB_8$$: `1LB1RZ_0LC1LC_0LD0LC_1LE1RA_0LF0LE_1LG1RD_0LH0LG_1RH1RF` ([on bbchallenge](https://bbchallenge.org/1LB1RZ_0LC1LC_0LD0LC_1LE1RA_0LF0LE_1LG1RD_0LH0LG_1RH1RF))
* $$BB_9$$: `1LD1LB_1LZ1LA_0LB1LD_0LE0LD_1LF1RC_0LG0LF_1LH1RE_0LI0LH_1RI1RG` ([on bbchallenge](https://bbchallenge.org/1LD1LB_1LZ1LA_0LB1LD_0LE0LD_1LF1RC_0LG0LF_1LH1RE_0LI0LH_1RI1RG))
* $$BB_{10}$$: `1LB1RZ_0LC1LC_0LD0LC_1LE1RA_0LF0LE_1LG1RD_0LH0LG_1LI1RF_0LJ0LI_1RJ1RH` ([on bbchallenge](https://bbchallenge.org/1LB1RZ_0LC1LC_0LD0LC_1LE1RA_0LF0LE_1LG1RD_0LH0LG_1LI1RF_0LJ0LI_1RJ1RH))

Note:

1. I have not normalized these to TNF. Instead I've left them with the same directions and state order as Green described them for ease of comparison.
2. I am using state `Z` in all of these as the halt state. `H` is not a halt state, but instead the 8th state.


## Class G Machines

In order to motivate the $$G_n$$ component, Green defines a general specification for TMs: **Class G Machines**. A TM, `M`, as a Class G Machine (with entry state $$M_{in}$$ and exit state $$M_{out}$$) if it conforms to the following behavioral specification:

$$\begin{array}{l}
  0^\infty & 1^k & M_{in} > & 0 & \to & 0^\infty & 1^m & M_{in} > \\
           &     & M_{in} > & 1 & \to &          & 1   & M_{out} > \\
\end{array} $$

In words:
- When started in its entry state ($$M_{in}$$) on a `0`
  * with a string on $$k \ge 0$$ `1`s to its left (and nothing else),
  * it performs some computation without ever traveling to the right of the initial location
  * until it eventually leaves to the right in the same state ($$M_{in}$$)
  * with a string `1`s to its left (and nothing else).
- When started in its entry state ($$M_{in}$$) on a `1`,
  * it moves immediately to the right and enters the exit state ($$M_{out}$$).

We will say that `M` computes the function $$M(k) = m$$.


### Recurrence

Note: An immediate consequence of the above specification is that:

$$\begin{array}{l}
  0^\infty & 1^k & M_{in} > & 0^r & 1 & \to & 0^\infty & 1^{M^r(k) + 1} & M_{out} > \\
\end{array} $$


### G Components

Each $$G_n$$ component is a Class G Machine (with entry state $$Q_n$$ and exit state $$Q_{n+2}$$). We can see this via induction.


#### Base Case

We can see directly that $$G_1$$

|  State      |  0              |  1              |
| :---------: | :-------------: | :-------------: |
| $$Q_1$$     | $$1 R Q_1$$     | $$1 R Q_3$$     |

is a Class G Machine (with entry state $$Q_1$$, exit state $$Q_3$$) which computes the function:

$$ G_1(k) = k + 1 $$


#### Inductive Step

We can build $$G_{n}$$ by simply adding two states to $$G_{n-2}$$:

|  State      |  0              |  1              |
| :---------: | :-------------: | :-------------: |
| $$Q_n$$     | $$1 L T_n$$     | $$1 R Q_{n+2}$$ |
| $$T_n$$     | $$0 L Q_{n-2}$$ | $$0 L T_n$$     |

Now, assuming that $$G_{n-2}$$ is a Class G Machine (with entry state $$Q_{n-2}$$, exit state $$Q_{n}$$), we will prove that $$G_{n}$$ is a Class G Machine (with entry state $$Q_{n}$$, exit state $$Q_{n+2}$$):

First, the easy part:

$$\begin{array}{l}
  Q_n > & 1 & \to & 1 & Q_{n+2} > \\
\end{array} $$

in a single step.

Second, we will use the inductive assumption which tells us that

$$\begin{array}{l}
  0^\infty & 1^k & Q_{n-2} > & 0^r & 1 & \to & 0^\infty & 1^{G_{n-2}^r(k) + 1} & Q_n > \\
\end{array} $$

in order to prove:

$$\begin{array}{l}
  0^\infty & 1^k & Q_n > & 0 & \to & 0^\infty & 1^k & < T_n & & 1 \\
           &     &       &   & \to & 0^\infty &     & < T_n & 0^k & 1 \\
           &     &       &   & \to & 0^\infty &     & < Q_{n-2} & 0^{k+1} & 1 \\
           &     &       &   &  =  & 0^\infty &     & Q_{n-2} > & 0^{k+2} & 1 \\
           &     &       &   & \to & 0^\infty & 1^{G_{n-2}^{k+2}(0) + 1} & Q_n > \\
\end{array} $$

thus proving that $$G_n$$ is a Class G Machine and also that

$$ G_n(k) = G_{n-2}^{k+2}(0) + 1 $$


### Fast-growing Hierarchy

Thus we can see that $$G_n$$ are all Class G Machines which compute a [fast-growing hierarchy](https://en.wikipedia.org/wiki/Fast-growing_hierarchy) of functions. The sequence begins with:

|          |  # States  |  Function          |
| :------: | :--------: | :----------------- |
| $$G_1$$  |          1 | $$G_1(k) = k + 1$$ |
| $$G_3$$  |          3 | $$G_3(k) = G_1^{k+2}(0) + 1 = k + 3$$ |
| $$G_5$$  |          5 | $$G_5(k) = G_3^{k+2}(0) + 1 = 3k + 7$$ |
| $$G_7$$  |          7 | $$G_7(k) = G_5^{k+2}(0) + 1 = \frac{7 \cdot 3^{k+2} - 5}{2} > 3^k$$ |
| $$G_9$$  |          9 | $$G_9(k) = G_7^{k+2}(0) + 1 > 3 \uparrow\uparrow k$$ |


## Setup Components

While the sequence of $$G_n$$ machines above compute impressively large functions, they need a little help when starting from a blank tape. This is where the Setup Components $$S_3$$ and $$S_4$$ come in. These setup components are very clever, they find a way to efficiently use the $$G_n$$ components to initialize the tape for themselves.


### Three-State Setup

Starting from any $$G_n$$, we can produce the $$n+3$$ state Green Machine $$BB_{n+3}$$ by adding the $$S_3$$ component:

|  State            |  0          |  1      |
| :---------------: | :---------: | :-----: |
|  $$A = Q_{n+2}$$  | $$1LB$$     | $$1RZ$$ |
|  $$B$$            | $$0LT$$     | $$1LT$$ |
|  $$T$$            | $$0 L Q_n$$ | $$0LT$$ |

Then we can see, that, starting in state `A` on a blank tape we get the trajectory:

$$\begin{array}{l}
  0^\infty & <A & 0^\infty & \to & 0^\infty & & < Q_n & 001 & 0^\infty \\
           &    &          &  =  & 0^\infty & & Q_n > & 0001 & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_n^3(0) + 1} & Q_{n+2} > & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_n^3(0) + 1} & <B & 1 & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_n^3(0)} & <T & 11 & 0^\infty \\
           &    &          & \to & 0^\infty & & <T & 0^{G_n^3(0)} & 11 & 0^\infty \\
           &    &          & \to & 0^\infty & & < Q_n & 0^{G_n^3(0) + 1} & 11 & 0^\infty \\
           &    &          &  =  & 0^\infty & & Q_n > & 0^{G_n^3(0) + 2} & 11 & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_n^{G_n^3(0) + 2}(0) + 1} & Q_{n+2} > & 1 & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_n^{G_n^3(0) + 2}(0) + 2} & Z> & 0^\infty \\
\end{array} $$

Which halts with exactly $$BB_{n+3} = G_n^{G_n^3(0) + 2}(0) + 2$$ `1`s on the tape (this TMs "score").

Note that:

$$G_{n+2}(k) = G_n^{k+2}(0) + 1$$

so we can rewrite these scores as

$$BB_{n+3} = G_n^{G_n^3(0) + 2}(0) + 2 = G_n^{G_{n+2}(1) + 1}(0) + 2 = G_{n+2}(G_{n+2}(1) - 1) + 1$$


### Four-State Setup

Starting from any $$G_n$$, we can produce the $$n+4$$ state Green Machine $$BB_{n+4}$$ by adding the $$S_4$$ component:

|  State            |  0          |  1      |
| :---------------: | :---------: | :-----: |
|  $$A$$            | $$1LT$$     | $$1LB$$ |
|  $$B$$            | $$1LZ$$     | $$1LA$$ |
|  $$C = Q_{n+2}$$  | $$0LB$$     | $$1LT$$ |
|  $$T$$            | $$0 L Q_n$$ | $$0LT$$ |


#### Prerequisites

This setup is more subtle to prove. It actually depends upon the fact that $$G_n(k)$$ is a "parity swapping function", in other words that:

$$\begin{array}{l}
  G_n(2k)   & \equiv & 1 & \pmod{2} \\
  G_n(2k+1) & \equiv & 0 & \pmod{2} \\
\end{array} $$

This turns out to be true for all $$G_n(k)$$ (which can be proven easily by induction).

Specifically, we use this fact below in the following transitions:

$$\begin{array}{l}
  1^{G_{n+2}(0)} & <B & \to & <A & 1^{G_{n+2}(0)} \\
  1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))} & <B & \to & <B & 1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))} \\
\end{array} $$

Since $$G_{n+2}(0)$$ is odd and $$G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))$$ is even.

We also need to prove a general rule first:

$$\begin{array}{l}
  0^\infty & 1^k & C> & 1 & \to & 0^\infty & 1^k & <T & 1 \\
           &     &    &   & \to & 0^\infty & <T & 0^k & 1 \\
           &     &    &   & \to & 0^\infty & < Q_n & 0^{k+1} & 1 \\
           &     &    &   &  =  & 0^\infty & Q_n > & 0^{k+2} & 1 \\
           &     &    &   & \to & 0^\infty & 1^{G_n^{k+2}(0) + 1} & Q_{n+2} > \\
           &     &    &   &  =  & 0^\infty & 1^{G_{n+2}(k)} & C> \\
\end{array} $$

Applying this recursively we get:

$$\begin{array}{l}
  0^\infty & 1^k & C> & 1^r & \to & 0^\infty & 1^{G_{n+2}^r(k)} & C> \\
\end{array} $$


#### Behavior

Finally, we can see, that, starting this TM in state `A` on a blank tape we get the trajectory:

$$\begin{array}{l}
  0^\infty & <A & 0^\infty & \to & 0^\infty & & < Q_n & 01 & 0^\infty \\
           &    &          &  =  & 0^\infty & & Q_n > & 001 & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_{n+2}(0)} & C> & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_{n+2}(0)} & <B & 0^\infty \\
           &    &          & \to & 0^\infty & & <A & 1^{G_{n+2}(0)} & 0^\infty \\
           &    &          & \to & 0^\infty & & < Q_n & 0 & 1^{G_{n+2}(0) + 1} & 0^\infty \\
           &    &          &  =  & 0^\infty & & Q_n > & 00 & 1^{G_{n+2}(0) + 1} & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_{n+2}(0)} & C> & & 1^{G_{n+2}(0)} & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))} & C> & 0^\infty \\
           &    &          & \to & 0^\infty & 1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))} & <B & 0^\infty \\
           &    &          & \to & 0^\infty & & <B & 1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0))} & 0^\infty \\
           &    &          & \to & 0^\infty & & <Z & 1^{G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0)) + 1} & 0^\infty \\
\end{array} $$

Which halts with exactly $$BB_{n+4} = G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0)) + 1$$ `1`s on the tape (this TMs "score").

Once again, we can simplify:

$$BB_{n+4} = G_{n+2}^{G_{n+2}(0)}(G_{n+2}(0)) + 1 = G_{n+2}^{G_{n+2}(0) + 1}(0) + 1 = G_{n+4}(G_{n+2}(0) - 1)$$


## Summary

Putting this all together, Green defined components $$G_n$$ which compute a fast growing hierarchy of functions and two different setup components $$S_3$$ and $$S_4$$ which can be added on top in order to produce a sequence of TMs $$BB_n$$ which start on a blank tape and leave large numbers of `1`s upon halting. Precisely,

$$\begin{array}{l}
  G_1(k) & = & k + 1 \\
  G_n(k) & = & G_{n-2}^{k+2}(0) + 1 \\
  \\
  BB_{2n} & = & G_{2n-1}(G_{2n-1}(1) - 1) + 1 \\
  BB_{2n+1} & = & G_{2n+1}(G_{2n-1}(0) - 1) \\
\end{array} $$


## Values and Bounds

We can compute the following exact values and (reasonably tight) bounds using Knuth up-arrow notation:

$$\begin{array}{l}
  G_1(k) & = & k + 1        && G_1(0) =  1 & G_1(1) =  2 \\
  G_3(k) & = & k + 3        && G_3(0) =  3 & G_3(1) =  4 \\
  G_5(k) & = & 3k + 7       && G_5(0) =  7 & G_5(1) = 10 \\
  G_7(k) & = & \frac{7 \cdot 3^{k+2} - 5}{2} > 3^{k+3}
                            && G_7(0) = 29 & G_7(1) = 92 \\
  G_9(k) & > & 3 \uparrow\uparrow (k + 3)
      && G_9(0) = \frac{7 \cdot 3^{31} - 3}{2} \\
  G_{11}(k) & > & 3 \uparrow\uparrow\uparrow (k + 3) \\

    & \vdots \\

  G_{2n+5}(k) & > & 3 \uparrow^n (k + 3) \\
\end{array} $$

And similarly for $$BB_n$$ values:

$$\begin{array}{lcrcl}
  BB_{ 4} & = &       7 \\
  BB_{ 5} & = &      13 \\
  BB_{ 6} & = &      35 \\
  BB_{ 7} & = & 22\,961
          & > & 3^{ 9} & = & 3^{3^2} \\
  BB_{ 8} & = & \frac{7 \cdot 3^{93} - 3}{2}
          & > & 3^{94} & > & 3^{3^4} \\
  BB_{ 9} & = & G_9(28)
          & > & 3 \uparrow\uparrow 31
          & > & 3 \uparrow\uparrow 3 \uparrow\uparrow 2 \\
  BB_{10} & > & G_9(3 \uparrow\uparrow 4)
          &&
          & > & 3 \uparrow\uparrow 3 \uparrow\uparrow 4 \\

    & \vdots \\

  BB_{2n+5} & > & G_{2n+5}(3 \uparrow^{n-1} 3)
            & > & 3 \uparrow^n (3 \uparrow^{n-1} 3)
            & = & 3 \uparrow^n 3 \uparrow^n 2 \\
  BB_{2n+6} & > & G_{2n+5}(3 \uparrow^n 4)
            &&
            & > & 3 \uparrow^n 3 \uparrow^n 4 \\
\end{array} $$


## Green's Champions

When Green published his paper in 1964, all $$BB_n$$ for $$n \ge 6$$ were Busy Beaver champions. However, over the years several have been slowly chipped away. According to Pascal Michel's [Historical survey of Busy Beavers](https://bbchallenge.org/~pascal.michel/ha.html):

* $$BB_6$$ was surpassed in 1972 by Donald Lynn: $$\Sigma(6) \ge 42$$
* $$BB_7$$ was surpassed in 1990 by Marxen and Buntrock: $$\Sigma(6) \ge 136\,612$$ and
* $$BB_8$$ was surpassed in 2000 by Marxen and Buntrock: $$\Sigma(6) > 1.4 \times 10^{60}$$

Yet, beyond this I do not know of any TMs beating Green's Machines until finally, they are eclipsed by [Daniel Nagaj's massive BB(16) > Graham champion]({% post_url 2022-07-11-bb-16-graham %}) (since $$BB_{16} \ll g_2 \ll G$$).

| Green Machine | Busy Beaver Bound |
| :------------ | :---------------- |
| $$BB_{ 9}$$   | $$ \Sigma(9) > 3 \uparrow\uparrow 31 > 10 \uparrow\uparrow 30 $$ |
| $$BB_{10}$$   | $$ \Sigma(10) > 3 \uparrow\uparrow 3 \uparrow\uparrow 4 > 10 \uparrow\uparrow 10^{10^{12}} $$ |
| $$BB_{11}$$   | $$ \Sigma(11) > 3 \uparrow\uparrow\uparrow 3 \uparrow\uparrow\uparrow 2 > 10 \uparrow\uparrow\uparrow 10^{12} $$ |
| $$BB_{12}$$   | $$ \Sigma(12) > 3 \uparrow\uparrow\uparrow 3 \uparrow\uparrow\uparrow 4 $$ |
| $$BB_{13}$$   | $$ \Sigma(13) > 3 \uparrow^4 3 \uparrow^4 2 $$ |
| $$BB_{14}$$   | $$ \Sigma(14) > 3 \uparrow^4 3 \uparrow^4 4 $$ |
| $$BB_{15}$$   | $$ \Sigma(15) > 3 \uparrow^5 3 \uparrow^5 2 $$ |

Do any of you know of any better results in this range? Let me know in the new comments system below (just set up today!).


## Footnotes

[^green64]: Milton W. Green. "A Lower Bound on Rado's Sigma Function for Binary Turing Machines", _Proceedings of the IEEE Fifth Annual Symposium on Switching Circuits Theory and Logical Design_, 1964, pages 91–94, doi: [10.1109/SWCT.1964.3](https://doi.org/10.1109%2FSWCT.1964.3).
