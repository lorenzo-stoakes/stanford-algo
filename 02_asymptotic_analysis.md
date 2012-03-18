Design and Analysis of Algorithms I - Asymptotic Analysis
=========================================================

Big-Oh Notation
---------------

### English Definition ###

Let:-

    T(n) = function on n = 1, 2, 3

Usually the worst-case running time of an algorithm

Question: When is T(n) = O(f(n))?

Answer: If eventually (for all sufficiently large n), T(n) is bounded above by a constant
multiple of f(n).

Looking at a graphical representation:-

<img src="http://codegrunt.co.uk/images/algo/2-big-oh-notation-1.png" />

Here we see that above a certain n, 2f(n) is always greater than T(n).

Our formal definition:-

T(n) = O(f(n)) if and only if there exist constants c, n\_0 > 0 such that T(n) <= c.f(n) for all
n >= n\_0.

If we go back to the image and look for c and n_0 we have:-

<img src="http://codegrunt.co.uk/images/algo/2-big-oh-notation-2.png" />

We can consider this in terms of a game, where we pick c and n_0, and our opponent can pick any
n larger than n\_0 and see if they can have T(n) come below n\_0. T(n) is O(f(n)) only if you
can always win with f(n).

Note: c, n\_0 *cannot* depend on n.

Basic Examples
--------------

Some simple examples, considering whether big-oh really does suppress constants and lower-order
terms as we want it to.

### Example #1 ###

Claim: If:-

    [; T(n) = a_k n^k + ... + a_1n + a_0 ;]

Then:-

    [; T(n) = O(n^k) ;]

Proof:-

Choose:-

    [; n_0 = 1 ;]

    [; c = |a_k| + |a_{k-1}| + ... + |a_1| + |a_0| ;]

Need to show that:-

    [; \forall n \geq 1, T(n) \leq c.n^k ;]

We have, for every n >= 1:-

    [; T(n) \leq |a_k| n^k + ... + |a_1|n + |a_0| ;]

This is an upper bound as the absolute coefficients ensure that, given n >= 1, T(n) is only
going to be less than or equal to these values.

We can take advantage of this being an upper bound and make our life easier by replacing each n
with n^k, so:-

    [; T(n) \leq |a_k| n^k + ... + |a_1|n^k + |a_0| n^k ;]
    [; = c . n^k ;]

Thus the proof is made.

We've essentially reverse-engineered these constants. The subsequent optional examples section
discusses this further.

### Example #2 ###


