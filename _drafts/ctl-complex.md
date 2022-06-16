
In order to explain why this is useful, let me give an example I recently analyzed:

|     |  0  |  1  |
| :-: | :-: | :-: |
|  A  | 1RB | 1RA |
|  B  | 1LC | 0RF |
|  C  | 1RA | 0LD |
|  D  | 0LC | 0LE |
|  E  | 1LD | 0RA |
|  F  | 1RE | 1RZ |

And let's consider the set

 * <code>C = 0<sup>inf</sup> 1 <E (0101)* (00|1)* 0<sup>inf</sup></code>

Let me make sure my (ad hoc) notation is clear here. This is a configuration where the TM is in state `E` reading a `1`, to the left is all `0`s, to the right is a sequence of (0 or more) repetitions of the `0101` followed by a sequence of (0 or more) where each element of that sequence is either `00` or `1`, followed by infinitely many `0`s at the right tail.

So, for example, the configuration <code>0<sup>inf</sup> 1 <E 0101 111 00 1 0<sup>inf</sup></code> would match, but <code>0<sup>inf</sup> 1 <E 0101 111 <b>0</b> 1 0<sup>inf</sup></code> would not (the lone 0 is not allowed).

Now, it turns out that you can prove the following rules:

 * Rule 0: <code>0000 1 <E 0101<sup>n</sup> 1<sup>3k</sup> 00 -> 1 <E 0101 1 0<sup>4n</sup> 001<sup>k</sup> 1</code>
 * Rule 1: <code>0000 1 <E 0101<sup>n</sup> 1<sup>3k+1</sup> 00 -> 1 <E 0101<sup>n+1</sup> 001<sup>k</sup> 1</code>
 * Rule 2: <code>0000 1 <E 0101<sup>n</sup> 1<sup>3k+2</sup> 00 -> 1 <E 0101 1<sup>4n+2</sup> 001<sup>k</sup> 1</code>

(I will leave the proofs as an exercise to the reader :) ).

Next, note that for any config in <code>C = 0<sup>inf</sup> 1 <E (0101)* (00|1)* 0<sup>inf</sup></code> it can be written as <code>0<sup>inf</sup> 1 <E 0101<sup>n</sup> 1<sup>m</sup> 00 X 0<sup>inf</sup></code> where `X` matches `(00|1)*`. In other words, we can pull out all the leading `1`s before the first `00` in the `(00|1)*` section (if the `(00|1)*` section has no `00`s, then we can just pull two `0`s off of the infinite right tail.). Finally, consider the 3 options for the remainder of `m` modulo 3:

 * <code>0<sup>inf</sup> 1 <E 0101<sup>n</sup> 1<sup>3k</sup> 00 X 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> 1 <E 0101 1 0<sup>4n</sup> 001<sup>k</sup> 1 X 0<sup>inf</sup></code> by Rule 0
 * <code>0<sup>inf</sup> 1 <E 0101<sup>n</sup> 1<sup>3k+1</sup> 00 X 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> 1 <E 0101<sup>n+1</sup> 001<sup>k</sup> 1 X 0<sup>inf</sup></code> by Rule 1
 * <code>0<sup>inf</sup> 1 <E 0101<sup>n</sup> 1<sup>3k+2</sup> 00 X 0<sup>inf</sup></code> -> <code>0<sup>inf</sup> 1 <E 0101 1<sup>4n+2</sup> 001<sup>k</sup> 1 X 0<sup>inf</sup></code> by Rule 2

And finally, note that all of the right sides are in `C`.
