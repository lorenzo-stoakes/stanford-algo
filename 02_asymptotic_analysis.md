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

This is something of a non-example - i.e. something is *not* big-oh of something else.

Claim: For every k >= 1 n^k is *not* O(n^{k-1}).

Proof: By contradiction.

Let's assume n^k = O(n^{k-1})

Then:-

    [; \exists c, n_0 > 0 ;]

Such that:-

    [; n^k \leq c.n^{k-1} \forall n \geq n_0 ;]

If we cancel n^{k-1} from both sides, then we have:-

    [; n \leq c \forall n \geq n_0 ;]

Which is patently not true, e.g. n = c + 1.

Big Omega and Theta
-------------------

Big Oh is the most important and ubiquitous concept in this analysis, but for completeness we
look at Big Omega and Big Theta.

### Omega Notation ###

Definition:

    [; T(n) = \Omega(f(n)) ;]

iff there exist constants c, n_0 such that:-

    [; T(n) \geq c . f(n) \forall n \geq n_0 ;]

Looking at this pictorially:-

<img src="http://codegrunt.co.uk/images/algo/2-big-oh-notation-3.png" />

### Theta Notation ###

Definition:-

    [; T(n) = \Theta(f(n)) ;]

iff:-

    [; T(n) = O(f(n)) ;]
    [; T(n) = \Omega(f(n)) ;]

There exist constants:-

    [; c_1, c_2, n_0 ;]

Such that:-

    [; c_1 f(n) \leq T(n) \leq c_2 f(n) ;]

For all:-

    [; n \geq n_0 ;]

We typically don't need to make a stronger statement that the use of theta would imply, as
we're typically interested in upper bounds, and saying O(f(n)) suffices.

E.g.:-

    [; T(n) = \frac{1}{2}n^2 + 3n ;]

We can say:-

    [; T(n) = \Omega(n) ;]
    [; T(n) = \Theta(n^2) ;]
    [; T(n) = O(n^3) ;]

Even though the lower and upper bounds are not great choices.

### Little-Oh Notation ###

This is not used very often, but is worth covering.

Definition :-

    [; T(n) = o(f(n)) ;]

iff for all constants c > 0 there exists a constant n_0 such that:-

    [; T(n) \leq c . f(n) \forall n \geq n_0 ;]

This is quite a lot stronger than the big-oh definition, as we assert this for *all* constants,
rather than one particular one.

Exercise:-

Show that:-

    [; \forall k \geq 1, n^{k-1} = o(n^k) ;] 
