---
layout: post
title: Up-arrow Comparisons
tags: big-number-notation
---

How do you compare two numbers in [up-arrow notation]({% post_url 2022-06-25-ext-up-notation %})? Say you have two numbers: $$a \uparrow^n x$$ vs. $$b \uparrow^m y$$. Which one is bigger? We can't simply "compute" them, even for small parameter choices like $$3 \uparrow^3 3$$ these numbers are so astronomically large that there are not enough atoms in the universe even to write all their digits! Instead we need some theory. Some reasonable rules of thumb are:

 * The operator "level" is most important. In general:

$$ n > m \to a \uparrow^n x > b \uparrow^m y $$

 * The iteration count is the second most important. In general:

$$ x \gg y \to a \uparrow^n x > b \uparrow^n y $$

 * The base is the least important.

$$ a > b \to a \uparrow^n x > b \uparrow^n x $$

However, these "rules" are not strictly true for small enough values. For example: $$2 \uparrow^{138} 2 = 4 < 3 \uparrow 2$$, so obviously operator "level" does not improve things in this case. What we really want is an algorithm that takes two numbers written in up-arrow notation and figures out how to compare them.


## Standard Form

Our technique will be to define a process for converting any up-arrow notation number into an (approximately equal) number in a standardized form and then a method for comparing two numbers in this standard form. Converting to standard form requires two steps:

 1. Converting base to a standard base (we'll use 10).
 2. Standardizing the up-arrow level ($$n$$ in $$\uparrow^n$$)
 3. Calculating the correct iteration count for this base and level.


## Exponentiation (↑)

We might as well start at the simplest level, basic exponentiation. Because here we already know how to convert between bases. Specifically, $$a^x = 10^{x \log_{10}(a)}$$. So, if we hae two numbers $$a^x$$ and $$b^y$$ in different bases (which are too large to compute directly) we can compare them by converting them to a common base. So we get $$10^{x \log_{10}(a)}$$ and $$10^{y \log_{10}(b)}$$ and comparing these reduces to the problem of comparing $$x \log_{10}(a)$$ vs. $$y \log_{10}(b)$$ and those can be computed directly.


## Tetration (↑↑)

Now let's consider Tetration. How do we convert a number $$a \uparrow \uparrow x$$ into base 10? Let's try an example:

$$ 3↑↑3 = 3^{3^3} = 10^{\log_{10}(3) \cdot 3^3} = 10^{10^{\log_{10}(3) \cdot 3 + \log_{10}(\log_{10}(3))}} \approx 10^{10^{1.1100}} \approx 10↑↑2.0453 $$

using the [fractional heights]({% post_url 2022-06-25-ext-up-notation %}#fractional-heights) tetration notation. Let's consider what happens as the height increases (let $$T = \log_{10}(3) \approx 0.4771$$ and $$S = \log_{10}(T) \approx -0.3214$$):

$$ 3↑↑4 = 3^{3^{3^3}} = 10^{T \cdot 3^{3^3}} = 10^{10^{T \cdot 3^3 + S}} \approx 10^{10^{12.5609}} \approx 10↑↑3.0410 $$

$$ 3↑↑5 = 3^{3^{3^{3^3}}} = 10^{T \cdot 3^{3^{3^3}}} = 10^{10^{T \cdot 3^{3^3} + S}} \approx 10^{10^{3,638,334,640,024}} \approx 10^{10^{10^{12.5609}}} \approx 10↑↑4.0410 $$

That's interesting. It looks like there's a pattern here. Specifically, it looks like (for $$n \ge 3$$),

$$ 3↑↑n \approx 10↑↑(n - 0.9690) $$

But this formula seems unbelievable, for example consider:

$$ 3↑↑(n+1) \approx 10↑↑(n + 0.0410) = 10^{10↑↑(n - 0.9690)} = 10^A $$

$$ 3↑↑(n+1) = 3^{3↑↑n} \approx 3^{10↑↑(n - 0.9690)} = 3^A $$

but that conclusion can't be right: $$ 10^A \not \approx 3^A $$. In fact, $$ 10^A > (3^A)^2 $$!

The problem here is that we have not defined $$\approx$$ precisely. When we say $$ 3↑↑5 \approx 10↑↑4.0410 $$ we do not mean that they are close in the traditional sense, for example. It does **not** mean that $$ \frac{\vert 3↑↑5 - 10↑↑4.0410 \vert}{3↑↑5} < \epsilon $$ for some small $$\epsilon$$ (In fact, I suspect that $$ \frac{\vert 3↑↑5 - 10↑↑4.0410 \vert}{3↑↑5} > 3↑↑5 $$!). Instead, what we really mean is that if $$ 3↑↑5 = 10↑↑y $$ then $$ y \approx 4.0410 $$. Or written another way $$ 3↑↑5 = 10↑↑ \; \sim 4.0410 $$. And in this notation $$ (10↑↑5)^{1000} = 10↑↑ \; \sim 5.0000 $$. In other words, these numbers are so big that even raising them to (relatively) large powers has almost no effect. And we can see this directly by observing that:

$$ (10↑↑5)^{1000} = (10^{10^{10^{10^{10}}}})^{1000} = 10^{1000 \cdot 10^{10^{10^{10}}}} = 10^{10^{3 + 10^{10^{10}}}} $$

And $$3 + 10^{10^{10}} \approx 10^{10^{10}}$$.

Returning to our example:

$$ 3↑↑5 = 3^{3^{3^{3^3}}} = 10^{T \cdot 3^{3^{3^3}}} = 10^{10^{S + T \cdot 3^{3^3}}} = 10^{10^{S + 10^{S + 10^{S + T \cdot 3}}}} = 10^{10^{S + 10^{S + 10^{\sim 1.1100}}}} = 10^{10^{S + 10^{\sim 12.8822 + S}}} $$

Now, $$S \approx -0.3214$$ so it will have a notable effect when added to $$\sim 12.8822$$ and shouldn't be ignored (if we want >2 digits of precision). So $$\sim 12.8822 + S = \sim 12.5609$$. However, it has a negligable effect when added to $$10^{\sim 12.8822 + S}$$ (unless you want >14 digits of precision).

### Fractional Tetration

For integer $$n$$ it is true that as $$n \to \infty$$, $$3↑↑n \to 10↑↑(n - d)$$ where $$d \approx 0.9690$$. Here is a graph of $$x$$ vs. $$y - x$$ where $$3↑↑x = 10↑↑y$$ where you can see that it asymptotically approaches the line $$y - x = d$$:

![Conversion from base 3 tetration to base 10 for integer heights](/assets/images/tetr_3_10_int.png)

However, something strange happens if you consider fractional heights:

![Conversion from base 3 tetration to base 10 for fractional heights](/assets/images/tetr_3_10_frac.png)

Here you can see that the instead of asymptotically approaching a line, it appears to approach a periodic cycle with period 1. I think that we are basically seeing the shortcomings of the definition we chose for fractional tetration heights. As I mentioned in [the article]({% post_url 2022-06-25-ext-up-notation %}#fractional-heights) where I defined fractional tetration height, this specific definition was explained on Wikipedia, however it is not differentiable at integer points and I think we are seeing the effect of that here.
