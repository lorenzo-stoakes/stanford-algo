Design and Analysis of Algorithms I - Master Method
===================================================

Motivation
----------

* We're studying the master method - which is a tool for analysing the running time of divide
  and conquer algorithms.

### Motivation ###

* More mathematical than previous lectures, but useful - a powerful tool that gives us a good
  indication of which algorithms are fast and which are slow. Novel algorithmic ideas often
  need mathematics to determine their performance.

* Consider the grade-school multiplication algorithm which uses O(n^2) operations to multiply
  two n-digit numbers.

### A Recursive Algorithm ###

Write:-

    [; x = 10^{n/2}a + b, y = 10^{n/2}c + d ;]

Where a, b, c and d are n/2- digit numbers.

So:-

    [; x.y = 10^nac + 10^{n/2}(ad + bc) + bd ;] (*)

Algorithm #1: Recursively compute ac, ad, bc, bd then compute (*) in obvious way.

We use a recurrence to analyse these sorts of recursive algorithm. We want to obtain an upper
bound on

T(n) = maximum number of operations this algorithm needs to multiply two n-digit numbers.

__Recurrence:__ Express T(n) in terms of running time of recursive calls.

__Base case:__ T(1) <= a constant.

__For all n>1:__ T(n) <= 4T(n/2) + O(n)

The second part of this equation refers to the work done outside of recursive calls,
i.e. grade-school addition.

### A Better Recursive Algorithm ###

Algorithm #2 (Gauss):

Recursively compute ac (1), bd (2) and (a+b)(c+d) (3),
Recall that ad + bc = (3) - (1) - (2).

T(n) <= 3T(n/2) + O(n)

For all n>1: T(n) <= 3T(n/2) + O(n)

Formal Statement
----------------

* Let's look at the master method's precise mathematical definition.

* It's essentially a black box for solving recurrences:-

__Input:__  Recurrence in a particular format
__Output:__ Upper bound on running time.

__Assumption:__ All subproblems have equal size.

### Recurrence Format ###

1. Base Case: T(n) <= a constant for all sufficiently small n. Pretty well always satisfied.

2. For all larger n:-

    [; T(n) \leq aT(n/b) + O(n^d) ;]

Where __a__ is the number of recursive calls (>= 1),
__b__ is the input size shrinkage factor (>1).
__d__ is the exponent in summing time of "combine step".

These are all constants, i.e. independent of __n__.

We aren't bothered by the constant contained in the combine step, this makes no difference.

### The Master Method ###

(Also known as the Master Theorem)

    [; \left\{\begin{matrix}
        O(n^d\log n)   & $ if $ a=b^d \\
        O(n^d)         & $ if $ a<b^d \\
        O(n^{\log_ba}) & $ if $ a>b^d
        \end{matrix}\right.
    ;]

Note that we're being sloppy about the base of the log in the first case, this is because here
the base simply doesn't matter as logs vary in a constant factor with respect to different
bases, whereas when the log is in an exponent, we do care as a constant factor can have a big
impact.
