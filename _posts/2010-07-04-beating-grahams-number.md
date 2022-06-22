---
layout: post
title: Beating Graham's Number
tags: big-number-notation busy-beaver
---

(This article was [original published](https://en.wikipedia.org/wiki/User:Sligocki/Beating_Graham%27s_number) on in Jul 2010 on my Wikipedia User page. I'm re-posting on my blog to bring my writing together in one place.)

This is a story of finding a specific busy beaver number which is greater than Graham's number. It might be called an essay, but since I am writing it as I work, I prefer story. I am aiming to develop a narrative similar to Knuth's "[Surreal Numbers: How Two Ex-Students Turned on to Pure Mathematics and Found Total Happiness](https://en.wikipedia.org/wiki/Surreal_number#History_of_the_concept)".

[Graham's number](https://en.wikipedia.org/wiki/Graham%27s_number), $$G$$, is an extremely large number that was claimed by [Martin Gardner](https://en.wikipedia.org/wiki/Martin_Gardner) and the [Guinness Book of World Records](https://en.wikipedia.org/wiki/Guinness_World_Records) as being the largest number ever used in a serious mathematical proof in 1980. It has become something of a standard for comparing extremely large numbers.

The [busy beaver function](https://en.wikipedia.org/wiki/Busy_beaver), $$\Sigma$$, is an example of an uncomputable function that grows faster than any computable function.

Clearly the busy beaver function will eventually surpass Graham's number, and I have proposed in the past that it will surpass Graham at a "relatively" small index, say $$\Sigma(100) > G$$. In fact, I believe that the index could be much smaller, say $$\Sigma(12) > G$$. However, as far as I know, nobody has placed such an upper bound on $$G$$. This story is an attempt to do that using some busy beaver value.


## Goodstein's function

My starting point for assaulting Graham's number, will be to create a Turing machine which computes [Goodstein's function](https://en.wikipedia.org/wiki/Goodstein%27s_function), $$H$$,[^goodstein-h] this function grows extremely fast, while still being computable. It can be shown that $$H(12) > G$$.

[^goodstein-h]: Rather than overload the letter "G", I will refer to this function as $$H$$ in reference to Kirby and Paris's relation of Goodstein's function to the Hydra game. Caicedo uses $$\mathcal{G}$$.


### Definitions

A Goodstein sequence is a sequence $$(a_2, a_3, ...)$$ whose elements are defined recursively based upon the initial value $$a_2$$ by the recursion:
* $$a_{k+1} = R_k^{k+1}(a_k) - 1$$ if $$a_k \ne 0$$ and
* $$a_{k+1} = 0$$ if $$a_k = 0$$

where $$R_k^m(a)$$ is the process of putting a in hereditary base $$k$$ notation and replacing all instances of $$k$$ with $$m$$.

$$H(n)$$ is the smallest index $$k$$ such that $$a_k = 0$$ given that $$a_2 = n$$. That is, it is the base that the Goodstein sequence vanishes at. Goodstein's theorem proves that this function is well-defined.[^goodstein-standard]

[^goodstein-standard]: Note that the standard definition of Goodstein's function is the length of the Goodstein sequence, this is, in fact, $$H(n) - 1$$.


### Notation

Our Turing machine will compute $$H$$ by simulating the Goodstein sequence until it vanishes. To do this we need to have a notation for the number $$a_k$$ in hereditary base $$k$$ notation. Consider as an example,

$$a_3 = 3^{3^3 \cdot 2 + 2} + 3^{3 + 1} \cdot 2 + 3 + 2$$

Now, my first thought was to simply represent this as a string of chars like we would in TeX, for example. Say as `3^3^3v*2+2v+3^3+1v*2+3+2` (where say ^ means step up into an exponent and v means step back down). But this is not organized very well for a program to parse through and has quite a bit of unnecessary information.

First of all, we don't really need to store all the 3s and then change them to 4s, etc. We could just store them as x's or something. In fact, we don't need to write them at all if we consider that the standard form is:

$$\alpha = 3^{\beta_k} \cdot c_k + ... + 3^{\beta_0} \cdot c_0$$

where Greek letters (α and β here) are hereditary notations and Latin letters ($$c$$ here) are integers less than the base. So a notation for α could be a list of ($$c$$, β) pairs, say:

* `[(2,0),(1,1),(2,[(1,0),(1,1)]), (1,[(2,0),(2,[(1,1)])])]` for the above $$a_3 = 3^{3^{3^1 \cdot 1} \cdot 2 + 3^0 \cdot 2} \cdot 1 + 3^{3^1 \cdot 1 + 3^0 \cdot 1} \cdot 2 + 3^1 \cdot 1 + 3^0 \cdot 2$$

This takes advantage of the fact that:
 1.  The notation stays the same when we change base using $$R_k^{k+1}(a_k)$$ and
 2.  The -1 step only affects the smallest term (which we place first here)

Note: This is quite reminiscent of [[Cantor normal form]] for ordinals and like with CNF, we could also expand the above equations out to get rid of multiplication, say like:

$$a_3 = 3^{3^{3^1} + 3^{3^1} + 3^0 + 3^0} + 3^{3^1 + 3^0} + 3^{3^1 + 3^0} + 3^1 + 3^0 + 3^0$$

which could be notated as, say:

`[0,0,1,[0,1],[0,1],[0,0,[1],[1]]]`

which simplifies the syntax significantly while still preserving the above facts. However it could also make for extremely long expressions when $$c$$ values are very large.

We'll keep both notation styles in mind as we consider the algorithm we'll use to simulate the Goodstein sequence.


### Algorithm

Our algorithm is a big loop. We start out with some base $$k$$ (say 2) and a Hereditary notation for the $$a_k$$ and then we:
 1.  Increment the base $$k \to k + 1$$
 1.  Subtract 1 from $$a_k$$ using new base
Until $$a_k$$ is 0. Then we halt leaving $$k$$ symbols on the tape.

Let's say we use the first notation and represent numbers in unary with k written out at the beginning. Thus $$a_3 = 3^{3^3 \cdot 2 + 2} + 3^{3 + 1} \cdot 2 + 3 + 2$$ from above would be `111[(11,),(1,1),(11,[(1,),(1,1)]),(1,[(11,),(11,[(1,1)])])]`.

Incrementing the base is trivial, clearly the computing $$a_k - 1$$ is the tricky part. Let's call that BigDecrement:

BigDecrement a:
1.  Find $$c$$ (the low coefficient)
    1. If no terms (i.e. there is no $$c$$) -> Exit (cannot decrement 0)
    2. If $$c = 0 \to$$ clear β and find next $$c$$ (Repeat at step 1)
    3. If $$c > 0$$ continue to step 2
2.  Decrement $$c$$
3.  While $$\beta > 0$$
    1. (*) Copy $$\beta$$ (the low exponent) before $$c$$
    2. Inside new $$\beta$$, BigDecrement $$\beta$$
    3. Copy $$k-1$$ before $$\beta$$ (as new $$c$$)
4.  Exit (Successfully decremented $$a$$)

It's a recursive function, so we'll have to make sure we can compute that appropriately to any depth of recursion.

The (*) indicates that Copying $$\beta$$ will be the difficult part here.


## Results from the Future

> Note: Section added on 22 Jun 2022.

I originally wrote this essay in 2010. I never ended up building a TM which beat Graham's number, but aparently I inspired some other folks to. This [CS Stack Exchange answer](https://cs.stackexchange.com/a/69476/2997) documents the history of improvement on $$BB$$ values beating Graham's number:

* On 19 Sep 2010: res001 claimed to have proven $$\Sigma(64) > G$$: [Blog](http://morethanazillion.blogspot.com/) [Summary](https://sites.google.com/site/res0001/surpassing-graham-s-number). In the summary, they directly reference this article.
* On 31 Mar 2013: "Deedlit11" claimed to have proven $$\Sigma(25) > G$$: [Googology Post](https://googology.fandom.com/wiki/User_blog:Deedlit11/Okay,_more_Turing_machines#A_New_Record!_Beating_Graham's_number_with_a_2-symbol_TM)
* There are uncited claims at proving $$\Sigma(23) > G$$ and $$\Sigma(22) > G$$.
* On 24 Jul 2016: "Wythagoras" claimed to have proven $$\Sigma(19) > G$$ and then $$\Sigma(18) > G$$: [Googology Post](https://googology.fandom.com/wiki/User_blog:Wythagoras/The_nineteenth_Busy_Beaver_number_is_greater_than_Graham's_Number!?useskin=oasis)

I have not had a chance to investigate any of these machines yet.


## Notes
