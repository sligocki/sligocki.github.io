---
layout: post
title: Notation for Very Large Numbers
tags: big-number-notation
---

(This article was [original published](https://en.wikipedia.org/wiki/User:Sligocki/Notation_for_very_large_numbers) on in Oct 2009 on my Wikipedia User page. I'm re-posting on my blog to bring my writing together in one place.)

What numbers are large?

One million is quite large, it takes a long time to count to it (over 5 days straight if you count 2 numbers a second). A trillion is gigantic (actually, I should probably say [tera](https://en.wikipedia.org/wiki/Tera-)-ntic), it would take 15,000 years to count to it at the same rate, even [Methuselah](https://en.wikipedia.org/wiki/Methuselah) would only have made it 7% of the way in a lifetime. A [quintillion](https://en.wikipedia.org/wiki/Names_of_large_numbers#Standard_dictionary_numbers) would take longer than the [age of the universe](https://en.wikipedia.org/wiki/Age_of_the_universe).

Yet in the [decimal](https://en.wikipedia.org/wiki/Decimal) [positional notation](https://en.wikipedia.org/wiki/Positional_notation) we can write these numbers as

 * 1,000,000
 * 1,000,000,000,000
 * 1,000,000,000,000,000,000

Even the total number of particles in the universe, probably one of the largest physical quantity considerable is not so bad in decimal. There are estimated to be around

100000000000000000000000000000000000000000000000000000000000000000000000000000000

elementary particles in the universe, certainly a large expression. Numbers of this size begin become cumbersome to write and people began using [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation), thus there are

$$10^{80}$$

particles in the universe, that hardly seems that large of a number. Now

$$10^{100000000000000000000000000000000000000000000000000000000000000000000000000000000}$$

that seems like a big number! In fact it could never be written out in decimal because it would have more digits than there are particles in the universe. But we can easily write this number a little more concisely as

$$10^{10^{80}}$$

and now, once again it does not appear so large.

Let us generalize and say that a number is `tower(0)` if it could be represented concisely in decimal, `tower(1)` as scientific notation, `tower(2)` as a number of the form $$10^{10^n}$$, etc.

Now, these "tower notations" are quite powerful, with each level up we can represent previously incomprehensible numbers. It might even seem that we could represent any number concisely in _some_ `tower(k)` notation. Clearly the sequence $$1, 10, 10^{10}, 10^{10^{10}}, \dots$$ is unbounded and the the $$k$$th element is clearly `tower(k)`, thus any number is less than one of these and thus concisely represented (or at least a bound is concisely represented we think). But the problem is that eventually $$k$$ itself becomes too large and no we get a number like

$$10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{
       10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{
       10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{
       10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10^{10
}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}}} $$

which cannot be concisely represented in any `tower(k)` notation because the tower itself is too large.

Of course, once again, the solution is clear, we can describe this simply as a tower with 80 tens in it or

$$\underbrace{10^{10^{~\cdot~^{~\cdot~^{~\cdot~^{10}}}}}}_{80 \mbox{ tens}}$$

and once again the notation is concise. [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth) suggested that this be represented by

$$ 10 \uparrow\uparrow 80 $$

We could keep going along this line of reasoning to develop higher and higher operations (and even name them if they had not already been). However, the question might be asked, why? and to what end. I do not mean that in a practical way (if you are concerned with practicality, you probably stopped reading at scientific notation), but at some point we may run out of raw curiosity to continue our futile quest to infinity. What might motivate us to pick up the gauntlet again? a challenge!

## Goodstein sequences

> See also my [article on Goodstein Sequences]({% post_url 2009-10-14-goodstein-sequences %})

In 1944, [Reuben Goodstein](https://en.wikipedia.org/wiki/Reuben_Goodstein) proposed a type of sequence that were eventually called [Goodstein sequences](https://en.wikipedia.org/wiki/Goodstein%27s_theorem) (by Kirby and Paris, 1982).

Start with $$x_1 = 2^{2^2 + 1} + 2^{2 + 1} + 1$$ in _hereditary base-2 notation_ (see [Definition of a Goodstein sequence](https://en.wikipedia.org/wiki/Goodstein%27s_theorem#Goodstein_sequences)) and we advance it to hereditary base-3 by simply changing all the 2s to 3s $$3^{3^3 + 1} + 3^{3 + 1} + 1$$, finally we subtract 1 and we have $$x_2 = 3^{3^3 + 1} + 3^{3 + 1}$$. Continuing we get

$$x_3 = 4^{4^4 + 1} + 4^{4 + 1} - 1 = 4^{4^4 + 1} + 3 \cdot 4^4 + 3 \cdot 4^3 + 3 \cdot 4^2 + 3 \cdot 4 + 3$$

$$x_4 = 5^{5^5 + 1} + 3 \cdot 5^5 + 3 \cdot 5^3 + 3 \cdot 5^2 + 3 \cdot 5 + 2$$

These sequences grow extremely quickly, yet, Goodstein proved that they always eventually reach 0. But, even more surprisingly, this cannot be proven in the methods of [Peano arithmetic](https://en.wikipedia.org/wiki/Peano_arithmetic)! (Kirby and Paris, 1982) In fact, Goodstein's theorem itself uses the [well orderedness](https://en.wikipedia.org/wiki/Well-order) of the [ordinals](https://en.wikipedia.org/wiki/Ordinal_number) to prove the eventual termination.

But the algorithm used to create the sequence does not seem too complicated, we might be tempted to think that we could figure out how long it would take to halt. [Ronald Graham](https://en.wikipedia.org/wiki/Ronald_Graham) might even [give you $25](http://garden.irmacs.sfu.ca/?q=op/termination_of_the_sixth_goodstein_sequence) if you can find the length of the 6th Goodstein sequence!

... to be continued ...
