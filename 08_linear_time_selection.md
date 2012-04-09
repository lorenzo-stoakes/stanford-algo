Design and Analysis of Algorithms I - Linear-Time Selection
===========================================================

Randomised Selection - Algorithm
--------------------------------

The selection problem is the problem of computing *order statistics* of an array, with
computing the median of an array being a special case.

The goal is the design and analysis of a very practical randomised algorithm to solve the
problem.

We can achieve a linear solution, i.e. O(n) where n is the length of the array.

### Prerequisites ###

Have watched:-

* QuickSort - Partitioning around a pivot.
* QuickSort - Choosing a good pivot.
* Probability Review, Part I. Random variables, expectation and linearity of expectation.

### The Problem ###

__Input:__ Array A with n distinct numbers and a number i in {1,2,...,n}.

__Output:__ ith order statistic i.e. the ith smallest element of the input array.

__Example:__ Median - the middle element. i = (n+1)/2 for n odd, i = n/2 for n even.

Median can be more useful than mean because the mean of an array because, for one, the median
can be affected by bad data, whereas the median is less likely to be affected.

### Reduction to Sorting ###

#### O(n log n) Algorithm ####

1. Apply MergeSort
2. Return ith element of sorted array.

We 'reduce' the problem from one problem we do not know the solution to, to another problem
which we *do* know how to solve.

The mantra of the algorithm designer comes into play again - can we do better?

__Fact:__ We can't sort any faster than O(n log n) [see optional video].

__Optional:__ O(n) time deterministic algorithm (using 'median of medians' pivot).

The randomised algorithm is more practical, however, as it has smaller constants than the
deterministic approach, and can be performed in-place.

### Partitioning Around a Pivot ###

We already know about partitioning around a pivot.

We rearrange around a chosen element so that elements to the left of the pivot is less than the
pivot, and items to the right are greater than the pivot.

How are we going to recurse the partition algorithm in order to solve the selection problem?

E.g. if we were looking for the 5th order statistic in an input array of length 10 + we
partitioned the array and the pivot ended up in the 3rd position of the partitioned array, then
we'd recurse into the 2nd half as we know, by definition, that the pivot is in its 'rightful'
position, and thus everything below it is, though unsorted, going to be the 2nd order statistic
or lower, and everything to the right of it will be the 4th order statistic or higher.

### Randomised Selection ###

    RSelect(array A, length n, order statistic i):
      0. if n = 1, return A[1]
      1. Choose pivot p from A uniformly at random.
      2. Partition A around p.
         let j = new index of p.
      3. If j = i return p.
      4. If j > i return RSelect(1st part of A, j-1, i).
      5. If j < i return RSelect(2nd part of A, n-j, i-j).

### Properties of RSelect ###

__Claim:__ RSelect is correct (guaranteed to output ith order statistic).

__Proof:__ By induction (like in optional QuickSort video).

__Running Time?:__ Depends on "quality" of the chosen pivots.

Worst case running time of RSelect is:-

    [; \Theta(n^2) ;]

__Key:__ Find pivot giving a "balanced" split.

__Best Pivot:__ The median (but this could be circular).

Would get recurrence T(n) <= T(n/2) + O(n), which is case 2 of the master method -> O(n) time.

__Hope:__ Random pivots is "pretty good" "often enough".

### Running Time of RSelect ###

__RSelect Theorem:__ For every input array of length n, the average running time of RSelect is
O(n).

* Holds for *every* input [no assumptions on data].

* Average is over random pivot choices made by the algorithm.

Randomised Selection - Analysis
-------------------------------

* Now we'll consider the mathematical analysis of this algorithm.

* Selection is a fundamentally easier problem than sorting.

### Proof 1: Tracking Progress via Phases ###

__Note:__ RSelect uses <= Cn operations outside of the recursive call [for some constant C>0]
(from partitioning).

__Notation:__ RSelect is in phase j if current array size, S, is:-

    [; (\frac{3}{4})^{j+1}n \leq S \geq (\frac{3}{4})^jn ;] (*)

The phase j essentially quantifies the number of times we've made 75% progress in the solution
to the problem.

We define X_j as the number of recursive calls during phase j.

__Note:___ Running time of RSelect less than or equal to:-

    [; \sum_{phases_j} X_j.c.(\frac{3}{4})^jn ;]

Where the value to the right of X_j is the work per phase of the subproblem.

Note both sides of this inequality are random variables, depending on which pivots get chosen.

### Proof 2: Reducing to Coin Flipping ###

X_j = # of recursive calls during phase j.

__Note:__ If RSelect chooses a pivot giving a 25-75 split (or better) then the current phase
ends, e.g. the new subarray length is at most 75% of the old length.

__Recall:__ (from QuickSort analysis) Probability of 25-75 split is 50%.

__So:__

    [; E[X_j] \leq ;] expected number of times you need to flip a fair coin to get one "heads".

Heads = good pivot, tails = bad pivot.

### Proof 3: Coin Flipping Analysis ###

Let N (geometric random variable with parameter 1/2) = number of coin flips until you see first
head.

__Note:__ E[N] = 1 + 1/2.E[N]

The 1 comes from this being the first coin flip, the 1/2 from the probability of tails, and the
E[N] from the further number of coin flips required in that case (given that this is a
memoryless process, i.e. it doesn't matter what we tossed on one occasion, it has no bearing on
the next.)

__Solution:__ E[N] = 2.

### Putting It All Together ###

Expected running time of RSelect <=
    [; E[Cn \sum_{phase_j}(\frac{3}{4})^jX_j] ;] (*)

Using linearity of expectation:-

    [; = Cn \sum_{phase_j}(\frac{3}{4})^jE[X_j] ;]

Using our analogy to coin flips:-

    [; \leq 2C_n \sum_{phase_j}(frac{3}{4})^j ;]

By observing that we have a geometric sum here, we notice that:-

    [; \sum_{phase_j}(\frac{3}{4})^j \leq \frac{1}{1-3/4} = 4;]

Thus we have:-

    [; \leq 8cn ;]
