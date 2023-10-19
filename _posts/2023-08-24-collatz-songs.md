---
layout: post-custom
title: Sing me a song, you're the Collatz man
tags: collatz
---

I just enjoyed an excellent YouTube video, [My honest attempt at the Collatz Conjecture](https://www.youtube.com/watch?v=8iJOTKMg5-k), made by Fernando Franco Félix (Fer / @Highly Entropic Mind) as part of the [third Summer of Math (#SoME3)](https://3blue1brown.substack.com/p/some3-begins). He also [wrote it up as a PDF](https://drive.google.com/file/d/1iydESf21HKPdSWggDgPs5IKb-iOc9xe-/view). This blog post is a response to that video and paper.

In the video, Fer takes us on a journey that we rarely get to see: Inside the process of exploration that he himself took while trying his hand at the Collatz Conjecture. And the journey is a wild and exciting one, transforming this from a number problem into a Game of Thrones tile game, leading up to what seems like it might be a proof of the Collatz Conjecture itself, only to be followed by a sober self-analysis of how much his own "proof" fails Scott Aaronson's [heuristic crackpot test](https://scottaaronson.blog/?p=304). It reminds me of Donald Knuth's novelette: [Surreal Numbers: How Two Ex-students Turned on to Pure Mathematics and Found Total Happiness](https://www.google.com/books/edition/Surreal_Numbers/ZUkwDc3FokgC?hl=en) (which I highly recommend to anyone reading this blog) except with more drama!

As a long-time Collatz enthusiast, I really enjoyed seeing someone share their authentic journey in grappling with this famous problem. In this blog post, I would like to share an abstraction that is similar to Fer's, but I think you might find simpler to work with. My hope here is to give another tool to folks who might have been inspired by Fer's approach to pursuing Collatz.


## Collatz by My Name

Let me start by laying out an alternative way of thinking about the Collatz problem. It is similar to Fer's, but I think it is simpler and easier to reason about. First, some definitions:

The Collatz function is often written as:

$$ C(n) = \begin{cases}
  n/2   & \text{if } n \equiv 0 \pmod{2} \\
  3n+1  & \text{if } n \equiv 1 \pmod{2} \\
\end{cases} $$

However, since the $$3n+1$$ case is guaranteed to be even, it is often convenient to use the reduced version that automatically divides that result by 2:

$$ D(n) = \begin{cases}
  \frac{n}{2}     & \text{if } n \equiv 0 \pmod{2} \\
  \frac{3n+1}{2}  & \text{if } n \equiv 1 \pmod{2} \\
\end{cases} $$

Note that this function can also be expressed as:

$$ \begin{array}{l}
  D & : & 2k   & \mapsto &  k \\
    &   & 2k+1 & \mapsto & 3k+2 \\
\end{array} $$

And this is the version that we will find most useful.


### Songs of Ice and Fire

Let us call $$2k \mapsto k$$ an `I`ce transition and $$2k+1 \mapsto 3k+2$$ a `F`ire transition (since they lower or raise the "temperature" respectively). Starting at any initial number, repeatedly applying $$D$$ will produce a sequence of these transitions. For example, starting at 13, we get the sequence:

$$ 13 \overset{F} \mapsto 20 \overset{I} \mapsto 10 \overset{I} \mapsto 5 \overset{F} \mapsto 8 \overset{I} \mapsto 4 \overset{I} \mapsto 2 \overset{I} \mapsto 1 $$

Looking only at the types of transitions this simplifies to the sequence `FIIFIII`. Let us call a sequence of `I`s and `F`s like this a "Song" (These are analogous to what Fer calls a "Constellation", but I am too invested in my Game of Thrones pun to use that term!) and furthermore, let's say that a song is "sung" by a number if that number follows the transitions from that song. So 13 sings `FIIFIII` (and it also sings `FIIF` and `FIIFIIIFIFI`, etc.).

This inspires a few questions: Is every song sung by some number? Given a song, can we find which numbers sing it?


### The Songs That Are Sung

Let's consider what happens when we apply $$D$$ twice:

$$ \begin{array}{l}
  D^2 & : & 4k   & \overset{I} \mapsto & 2k   & \overset{I} \mapsto &  k \\
      &   & 4k+1 & \overset{F} \mapsto & 6k+2 & \overset{I} \mapsto & 3k+1 \\
      &   & 4k+2 & \overset{I} \mapsto & 2k+1 & \overset{F} \mapsto & 3k+2 \\
      &   & 4k+3 & \overset{F} \mapsto & 6k+5 & \overset{F} \mapsto & 9k+8 \\
\end{array} $$

Therefore, we can see that every possible 2 letter song is sung and we have characterized exactly which numbers sing those songs. For example, every number $$4k+3$$ sings the song `FF`.

Extending one further we get:

$$ \begin{array}{l}
  D^3 & : & 8k   & \overset{I} \mapsto &  4k    & \overset{I} \mapsto &  2k    & \overset{I} \mapsto &   k \\
      &   & 8k+1 & \overset{F} \mapsto & 12k+ 2 & \overset{I} \mapsto &  6k+ 1 & \overset{F} \mapsto &  9k+ 2 \\
      &   & 8k+2 & \overset{I} \mapsto &  4k+ 1 & \overset{F} \mapsto &  6k+ 2 & \overset{I} \mapsto &  3k+ 1 \\
      &   & 8k+3 & \overset{F} \mapsto & 12k+ 5 & \overset{F} \mapsto & 18k+ 8 & \overset{I} \mapsto &  9k+ 4 \\
      &   & 8k+4 & \overset{I} \mapsto &  4k+ 2 & \overset{I} \mapsto &  2k+ 1 & \overset{F} \mapsto &  3k+ 2 \\
      &   & 8k+5 & \overset{F} \mapsto & 12k+ 8 & \overset{I} \mapsto &  6k+ 4 & \overset{I} \mapsto &  3k+ 2 \\
      &   & 8k+6 & \overset{I} \mapsto &  4k+ 3 & \overset{F} \mapsto &  6k+ 5 & \overset{F} \mapsto &  9k+ 8 \\
      &   & 8k+7 & \overset{F} \mapsto & 12k+11 & \overset{F} \mapsto & 18k+17 & \overset{F} \mapsto & 27k+26 \\
\end{array} $$

Therefore, every possible 3 letter song is also sung by infinitely many numbers. It turns out that this pattern continues and so for any song of length $$m$$, there exists a $$0 \le r < 2^m$$ such that all numbers $$n \equiv r \pmod{2^m}$$ sing that song.


### Who Sings a Song

In fact, given a song, we can directly compute the set of numbers which sing it using the following method:

Start with the empty song $$\epsilon$$, which does nothing:

$$ k_0 \overset{\epsilon} \mapsto k_0 $$

For each letter (`I` or `F`) in the song, starting from the beginning, update $$k$$ by making it either even ($$k_n = 2 k_{n+1}$$) or odd ($$k_n = 2 k_{n+1} + 1$$) whichever will lead to the right side of the expression being even (if the letter is `I`) or odd (if the letter is `F`).

For example, let's consider the song `FFFIFI`. We start with

$$ k_0 \overset{\epsilon} \mapsto k_0 $$

The first letter is `F`, so $$k_0 = 2 k_1 + 1$$ must be odd and we get:

$$ 2 k_1 + 1 \overset{\epsilon} \mapsto 2 k_1 + 1 \overset{F} \mapsto 3 k_1 + 2 $$

The second letter is also `F`, so $$3 k_1 + 2$$ must be odd, meaning $$k_1 = 2 k_2 + 1$$ also must be odd.

$$ 4 k_2 + 3 = 2 (2 k_2 + 1) + 1 \overset{F} \mapsto 3 (2 k_2 + 1) + 2 = 2 (3 k_2 + 2) + 1 \overset{F} \mapsto 3 (3 k_2 + 2) + 2 = 9 k_2 + 8 $$

$$ 4 k_2 + 3 \overset{FF} \mapsto 9 k_2 + 8 $$

Next, another `F`, so $$9 k_2 + 8$$ must be odd, meaning $$k_2 = 2 k_3 + 1$$ must be odd as well.

$$ 8 k_3 + 7 = 4 (2 k_3 + 1) + 3 \overset{FF} \mapsto 9 (2 k_3 + 1) + 8 = 2 (9 k_3 + 8) + 1 \overset{F} \mapsto 3 (9 k_3 + 8) + 2 = 27 k_3 + 26 $$

$$ 8 k_3 + 7 \overset{FFF} \mapsto 27 k_3 + 26 $$

The fourth letter is `I`, so $$27 k_3 + 26$$ must be even, meaning $$k_3 = 2 k_4$$ must be even as well.

$$ 16 k_4 + 7 = 4 (2 k_4) + 7 \overset{FFF} \mapsto 27 (2 k_4) + 26 \overset{I} \mapsto 27 k_4 + 13 $$

$$ 16 k_4 + 7 \overset{FFFI} \mapsto 27 k_4 + 13 $$

Next is `F`, so $$27 k_4 + 13$$ must be odd, so $$k_4 = 2 k_5$$ must be even.

$$ 2^5 k_5 + 7 = 16 (2 k_5) + 7 \overset{FFFI} \mapsto 27 (2 k_5) + 13 = 2 (27 k_5 + 6) + 1 \overset{F} \mapsto 3 (27 k_5 + 6) + 2 = 3^4 k_5 + 20 $$

$$ 2^5 k_5 + 7 \overset{FFFIF} \mapsto 3^4 k_5 + 20 $$

Finally, the last letter is `I`, so $$3^4 k_5 + 20$$ must be even, meaning that $$k_5 = 2 k_6$$ must be even as well.

$$ 2^6 k_6 + 7 = 2^5 (2 k_6) + 7 \overset{FFFIF} \mapsto 3^4 (2 k_5) + 20 \overset{I} \mapsto 3^4 k_5 + 10 $$

So, in conclusion, for all integers $$k$$:

$$ 2^6 k + 7 \xmapsto{FFFIFI} 3^4 k + 10 $$

In other words, the song `FFFIFI` is sung by all numbers of the form $$2^6 k + 7$$, and after being sung, those reach the numbers $$3^4 k + 10$$.


### Songs and Binary Representation

Note that numbers like $$2^6 k + 7$$ correspond to numbers which end in a specific sequence in base 2 ($$000111_2$$ in this case). So, thinking about these numbers in binary we can say simply that all numbers $$\dots 000111_2$$ sing the song `FFFIFI` and we can summarize this derivation with the following table:

|  Numbers  |  Song    |
| --------: | :------- |
|      ...1 | `F`      |
|     ...11 | `FF`     |
|    ...111 | `FFF`    |
|   ...0111 | `FFFI`   |
|  ...00111 | `FFFIF`  |
| ...000111 | `FFFIFI` |

Each letter added to a song refines the set of numbers that sing it by constraining one more bit (base-2 digit) in it's binary representation.


### Infinite Songs

Every number sings an infinite song since you can keep applying the $$D$$ function forever. But not all infinite songs are sung! In fact, the Collatz conjecture could be alternatively stated as: For any number $$n > 0$$, it's infinite song ends in `IF` repeated forever.

One example of a song that is never sung is `FFFFF...`. To see why, notice that the finite song $$F^m$$ is sung by numbers $$n \equiv 2^m - 1 \pmod{2^m}$$ (proof is left as an exercise to the reader). In order for a number to sing the infinite song $$F^\infty$$, it would have to sing all the finite songs $$F^m$$. But there is no positive number $$n \equiv 2^m - 1 \pmod{2^m}$$ for all $$m$$ since that would require $$n \ge 2^m - 1$$ which diverges. (However, if we extend ourselves to the negative integers as well, then in fact -1 does sing the infinite `FFFFF...` song!)


### Which Infinite Songs are Sung?

So, if you have a specific infinite song, how can we tell if it is ever sung? Well, let's consider the infinite song $$S_\infty$$ and all of it's finite prefixes $$S_m$$ of length $$m$$. In order for a number $$n$$ to sing $$S_\infty$$ it must sing all $$S_m$$. As we saw above, the numbers which sing $$S_m$$ are the numbers with a specific fixed $$m$$ bit suffix in their binary representation and by extending the song to $$S_{m+1}$$ that fixes one more bit.

As we continue this process, increasing the length of the finite songs, we fix more and more bits in the binary representation of numbers which sing that song. And so we can see, that eventually every bit becomes fixed and the infinite song $$S_\infty$$ corresponds to an infinite binary sequence $$\{0, 1\}^\infty$$. For a number to sing $$S_\infty$$, it's binary representation must have exactly that sequence.

But that binary sequence must have a very specific property in order for it to represent a finite integer: It must be eventually all `0`s! Because the binary sequence $$(b_0, b_1, \dots)$$ corresponds to the number $$\sum_{k=0}^\infty b_k 2^k$$ which only converges if, eventually, for all $$k > K$$, $$b_k = 0$$.

So, the infinite songs that are actually sung are exactly the infinite songs whose corresponding binary sequence is eventually all `0`s.

Of course, proving that a song's binary sequence is (or is not) eventually all `0`s is no easy task. AFAIK, there is no general method for doing this, all we can do is compute one more bit at a time.

As an aside: There does exist a number system in which all infinite binary sequences correspond to a "number". These are the [2-adic numbers](https://en.wikipedia.org/wiki/P-adic_number) and if you extend the Collatz problem to them, then all infinite songs are sung by a 2-adic number!


## Collatz by Fer's Name

We are finally ready to discuss Fer's abstraction. He focusses on numbers which he calls "nodes". These are the numbers $$x = C(a) = C(b)$$ for $$a \ne b$$. It turns out that these are exactly the numbers of the form $$x = C(2n+1) = 6n + 4$$.

Note that since $$6n + 4$$ is always even, it will always be followed by $$3n + 2$$. In fact, if you use the $$D$$ function it will skip right over the $$6n+4$$ values straight to the $$3n + 2$$ ones (at least coming from odd values). Therefore, in order to work more clearly with my abstraction I will consider the $$n$$-th node to be $$3n+2$$. These nodes are bijective to Fer's, just multiply them by two to get back to his.

He classifies nodes into three categories based on how it reaches the next node:

* Stark: $$n = 2k + 1$$, so $$3(2k+1) + 2 = 6k + 5 \overset{F} \mapsto 9k + 8 = 3 (3k+2) + 2$$
* Lannister: $$n = 4k$$, so $$3(4k) + 2 = 12k + 2 \overset{I} \mapsto 6k + 1 \overset{F} \mapsto 9k + 2 = 3 (3k) + 2$$
* Targaryen: $$n = 4k + 2$$, so $$3(4k+2) + 2 = 12k + 8 \overset{I} \mapsto 6k + 4 \overset{I} \mapsto 3k + 2$$

Using my terminology above:

* Stark nodes sing the song `F`
* Lannister nodes sing the song `IF`
* Targaryen nodes sing the song `II`

(I know, ironic that the Starks sing only of Fire and Targaryen's only of Ice! What sort of world is this?)


### Constellations

Fer defines a constellation as a sequence of Stark, Lannister and Targaryen tiles. So we can now see that every constellation corresponds directly to a song.

For example, the constellation `SSSLLLL` corresponds to the song `FFFIFIFIFIF`. And this song is sung by all numbers $$2^{11} k + 455$$ with

$$ 2^{11} k + 455 \xmapsto{FFFIFIFIFIF} 3^7 k + 488 $$

If we force the starting point to be a "node" (of form $$3n + 2$$) then we can rewrite this as:

$$ 3 (2^{11} k + 151) + 2 = 2^{11} (3k) + 455 \xmapsto{FFFIFIFIFIF} 3^7 (3k) + 488 = 3 (3^7 k + 162) + 2 $$

In other words: This constellation/song maps nodes $$2^{11} k + 151$$ to nodes $$3^7 k + 162$$. (Remember we are calling node $$n$$ the number $$3n + 2$$)


### Streaks

Fer defines a streak as a constellation with only one repeated tile. So the streaks are $$S^m$$, $$L^m$$ and $$T^m$$ for various lengths $$m$$. These correspond to the songs $$S^m = F^m$$, $$L^m = (IF)^m$$ and $$T^m = I^{2m}$$. Using my song of ice and fire terminology, we can see that a natural extention of the idea of a streak would be any repeated song, ex: $$(FFI)^m$$. I'll call these generalized streaks.

Streaks seem to mostly be of interest because they can allow us to specify the behavior of an entire class of constellations/songs. So:

* Targaryen streaks are easy to describe:

$$ 2^m k \overset{I^m} \mapsto k $$

* Stark streaks are also, once you notice that $$2k - 1 \overset{F} \mapsto 3k  - 1$$:

$$ 2^m k - 1 \overset{F^m} \mapsto 3^m k - 1 $$

* Lannister streaks are also solved by similarly noting that $$4k + 2 \overset{IF} \mapsto 3k + 2$$:

$$ 4^m k + 2 \overset{(IF)^m} \mapsto 3^m k + 2 $$

And we can use the same sort of tricks for some generalized streaks, like:

$$ 8^m k - 5 \overset{(FFI)^m} \mapsto 9^m k - 5 $$


#### Streaks on nodes

To compare these formulas to Fer's, we need to constrain ourselves to nodes again, then we get:

* Lannister streaks ($$L^m$$) send Node $$4^m k \to 3^m k$$ since:

$$ 3 (4^m k) + 2 = 4^m (3k) + 2 \overset{(IF)^m} \mapsto 3^m (3k) + 2 = 3 (3^m k) + 2 $$

* Stark streaks ($$S^m$$) send Node $$2^m k - 1 \to 3^m k - 1$$ since:

$$ 3 (2^m k - 1) + 2 = 2^m (3k) - 1 \overset{F^m} \mapsto 3^m (3k) - 1 = 3 (3^m k - 1) + 2 $$

Fer writes this as $$a_0 = 2^m b + 2^m - 1$$ and $$a_m = 3^m b + 3^m - 1$$ which is the same equation, just with $$k = b + 1$$ (presumably he chose this version so that all values $$b \ge 0$$ result in valid $$a > 0$$).

* Targaryen streaks ($$T^m$$) send Node $$4^m k + \frac{2}{3} (4^m - 1) \to k$$ since

$$ 3 (4^m k + \frac{2}{3} (4^m - 1)) + 2 = 4^m (3k + 2) \overset{I^{2m}} \mapsto 3k + 2 $$

Both my Lannister and Targaryen streak notations are the same as Fer's. As Fer notes, you do not need to be afraid of the $$\frac{2}{3}$$ factor in the Targaryen streak since $$4^m - 1$$ is always a multiple of 3!

Finally, if we consider these rules to all take $$a_0 \to a_m$$, Fer writes these equations out for $$a_m$$ in terms of $$a_0$$ (removing the $$k$$). So (note that the $$\lambda, \sigma, \tau$$ superscripts are just labels here, not exponents):

* Lannister streaks: $$a_m^\lambda = (\frac{3}{4})^m a_0^\lambda$$
* Stark streaks: $$a_m^\sigma = (\frac{2}{3})^m (a_0^\sigma + 1) - 1$$
* Targaryen streaks: $$a_m^\tau = (\frac{1}{4})^m (a_0^\tau + \frac{2}{3}) - \frac{2}{3}$$

Using this notation seems useful since it allows you to calculate the results of "multi streak drifting". So we can see what happens when we apply $$S^m L^r$$ by setting the $$a_0^\lambda = a_m^\sigma$$, resulting in:

$$ a_r^\lambda = (\frac{3}{4})^r [ (\frac{2}{3})^m (a_0^\sigma + 1) - 1 ]$$


### Fer's Fallacies

We have now come to a dramatic moment in Fer's argument: Section 4.2 in the paper or minute 29 in the video: "Infinite multi-streaks constellations require convergence for infinite
sums of integers". This is the part where Fer claims to have found a proof that we cannot have infinite constellations!

Now, let's start by saying that infinite constellations correspond to infinite songs and as we discussed above: there are infinite songs that are sung. In fact every starting number leads to an infinite song (because you can always apply the function $$D$$ again). Likewise, starting at every node, you get an infinite constellation simply by repeatedly applying the Stark, Lannister and Targaryen rules. So we know that infinite constellations exist.

As far as I can tell, Fer's argument goes like this: Let's say we have an infinite constellation and let's evaluate the equations for all the finite prefixes of that constellation. So as we discussed in the last section, let's say that the infinite constellation starts with  $$S^m L^r \dots$$, then we know that the equation for this prefix is:

$$ a_{m+r} = (\frac{3}{4})^r [ (\frac{2}{3})^m (a_0 + 1) - 1 ] = e^{r(\ln 3 - 2 \ln 2) + m(\ln 2 - \ln 3)} a_0 + \beta $$

then $$\lim_{m \to \infty} a_m$$ cannot converge because this would require an infinite series of integers to converge. But we already know that $$\lim_{m \to \infty} a_m$$ won't converge. This is a sequence of integers, so the only way for it to converge is if it becomes eventually constant. And to be eventually constant, it would mean that it reached a fixed point of $$D$$ and the only two fixed points of $$D$$ are $$D(0) = 0$$ and $$D(-1) = -1$$ which cannot be reached from positive numbers.

Instead, there are two possibilities, either the sequence $$a_m$$ eventually cycles or it grows forever. In both cases the sequence doesn't converge.

#### Starting Point Divergence?

At this point, I must admit that I cannot exactly put my finger on what Fer's argument is. I have made a good faith effort here to try and understand it and show the fallacy, but it is possible that I am missing what Fer really meant. As an example, rather than looking at $$a_m$$ which (as we discussed) clearly doesn't converge. We could look at what values $$a_0$$ can take as we increase the size of the prefix. As I discussed in the "Which Infinite Songs are Sung?" section, for some infinite songs, the starting point will diverge. Or, in other words, there will be no finite number which sings all the finite prefixes of this infinite song. But this is not true for all songs, as mentioned in that section, the songs which convert to binary sequences which are eventually all 0 each correspond to an infinite song that is sung.


### Westeros Function

One way to think about Fer's abstraction is that it defines a new Collatz-like function. I will call it the Westeros function:

$$ \begin{array}{l}
  W & : & 2k+1 & \mapsto & 3k+2 \\
    & : & 4k   & \mapsto & 3k   \\
    & : & 4k+2 & \mapsto &  k   \\
\end{array} $$

These three cases are exactly the three tiles:

* Stark: $$2k+1 \mapsto 3k+2$$
* Lannister: $$4k \mapsto 3k$$
* Targaryen: $$4k+2 \mapsto k$$

and the new conjecture is: starting at all $$n \ge 0$$, does $$W^m(n) = 0$$. In other words, if you keep applying $$W$$ do you eventually reach 0.

Now it should be clear the analogy between my "Songs of Ice and Fire" and Fer's "Constellations of Stark, Lannister and Targaryen". They are the same concept applied to a different Collatz-like function.

Is it helpful to abstract to the Westeros function? As far as I can tell, it is not. This is a more complicated function, it requires three different cases (instead of the two from the $$D$$ function) and I think that in general it makes most of the equations more complicated as well. And furthermore, I have not seen any advantages that compensate for this complexity. If you are interested in following this line of reasoning with respect to the Collatz, I'd recommend using the "Songs of Ice and Fire" abstraction instead as I've described throughout this article. But I look forward to being proven wrong when someone makes a great discovery using the Westeros function abstraction :)


## Finding Cycles

Finally, I thought it would be nice to end this post with a fun result. Specifically about how we can use the "Song of Ice and Fire" abstraction to find all cycles of a given length.

A number $$n$$ starts a cycle of length $$k$$ if and only if $$D^k(n) = n$$. So we can find all cycles of length $$k$$ by computing $$D^k$$ (like we did above for $$D^2$$ and $$D^3$$) and solve the linear equations. For example, the cycles of length 1 are:

$$ \begin{array}{l}
  n = 2k   & = D(2k)   & = k    & \to & k = 0  & \to & n = 0 \\
  n = 2k+1 & = D(2k+1) & = 3k+2 & \to & k = -1 & \to & n = -1 \\
\end{array} $$

Which are the $$0 \overset{I} \mapsto 0 $$ and $$ -1 \overset{F} \mapsto -1 $$ cycles.

Remember that

$$ \begin{array}{l}
  D^2 & : & 4k   & \mapsto &  k \\
      &   & 4k+1 & \mapsto & 3k+1 \\
      &   & 4k+2 & \mapsto & 3k+2 \\
      &   & 4k+3 & \mapsto & 9k+8 \\
\end{array} $$

so the cycles of length 2 are:

$$ \begin{array}{l}
  n = 4k   & = D(4k)   & = k    & \to & k = 0  & \to & n = 0 \\
  n = 4k+1 & = D(4k+1) & = 3k+1 & \to & k = 0  & \to & n = 1 \\
  n = 4k+2 & = D(4k+2) & = 3k+2 & \to & k = 0  & \to & n = 2 \\
  n = 4k+3 & = D(4k+3) & = 9k+8 & \to & k = -1 & \to & n = -1 \\
\end{array} $$

So, here we find the only known positive integer cycle:

$$ 1 \overset{F} \mapsto 2 \overset{I} \mapsto 1 $$

which also shows up starting at 2:

$$ 2 \overset{I} \mapsto 1 \overset{F} \mapsto 2 $$

and for cycles of length 3 we get:

$$ \begin{array}{l}
  n = 8k   & = D(8k)   & = k    & \to & k = 0  & \to & n = 0 \\
  n = 8k+1 & = D(8k+1) & = 9k+2 & \to & k = -1 & \to & n = -7 \\
  n = 8k+2 & = D(8k+2) & = 3k+1 & \to & k = -\frac{1}{5} \\
  n = 8k+3 & = D(8k+3) & = 9k+4 & \to & k = -1 & \to & n = -5 \\
  n = 8k+4 & = D(8k+4) & = 3k+2 & \to & k = -\frac{2}{5} \\
  n = 8k+5 & = D(8k+5) & = 3k+2 & \to & k = -\frac{3}{5} \\
  n = 8k+6 & = D(8k+6) & = 9k+8 & \to & k = -2 & \to & n = -10 \\
  n = 8k+7 & = D(8k+7) & = 27k+26 & \to & k = -1 & \to & n = -1 \\
\end{array} $$

which finds the known negative cycle of length 3 (3 times):

$$ -5 \overset{F} \mapsto -7 \overset{F} \mapsto -10 \overset{I} \mapsto -5 $$

Note that the rows where $$k$$ is not an integer mean that those cycles are impossible.

Of course, this search is rather inefficient since you must compute $$D^k$$ and consider each of its $$2^k$$ cases separately.
