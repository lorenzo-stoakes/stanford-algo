Design and Analysis of Algorithms I - Asymptotic Analysis
=========================================================

O(n log n) Algorithm for Counting Inversions I
----------------------------------------------

* We will get some more practice using divide-and-conquer algorithms, particularly looking at
  array inversions.

### The Divide and Conquer Paradigm ###

1. DIVIDE into smaller subproblems. Sometimes entirely conceptual, sometimes not.

2. CONQUER via recursive calls.

3. COMBINE solutions to subproblems into one for the original problem. A lot of the cleverness
of these algorithms happens here.

### The Problem ###

__Input:__ Array A containing the numbers 1,2,3,...,n in some arbitrary order.

__Output:__ Number of *inversions* = number of pairs (i, j) of array indices with i < j, and
A[i] > A[j].

### Examples and Motivation ###

Example: 1,3,5,2,4,6

Inversions: (3,2), (5,2), (5,4)

Pictorial representation:-

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-1.png" />

Motivations:-

* Numerical similarity measure between two ranked lists.

* What's the largest possible number of inversions a size 6 array can have? 15, this is 6C2, as
  in reverse order, everything is out of order with everything else.

### High-Level Approach ###

We can use a brute force approach, e.g.:-

    func inversions(ns []int) int {
    	ret := 0

    	for i := 0; i < len(ns); i++ {
    		for j := i+1; j < len(ns); j++ {
    			if ns[i] > ns[j] {
    				ret++
    			}
    		}
    	}

    	return ret
    }

However, this is:-

    [; \Theta(n^2) ;]

Can we do better? Yes, through divide and conquer.

Let's look at different *types* of inversion:-

(Note i < j)

* Left:  if i, j <= n/2
* Right: if i, j > n/2
* Split: if i <= n/2 < j

We can compute left/right inversions using recursion, however we need a separate subroutine for
split inversions.

### High-Level Algorithm ###

    Count(array A, length n)
        if n = 1 return 0
        else
          x = Count(1st half of A, n/2)
          Y = Count(2nd half of A, n/2)
          Z = CountSplitInv(A, n)
        return X + Y + Z

Goal: Implement CountSplitInv in linear - O(n) - time.

This means we can run the whole thing in O(n log n) time.

O(n log n) Algorithm for Counting Inversions II
-----------------------------------------------

The key challenge is counting the split inversions. The problem is that there might be a
quadratic number of split inversions, however we need to count these in linear time.

We're going to piggyback onto merge sort.

Key idea #2: Have recursive calls both count inversions *and sort*, i.e. piggy back on merge
sort.

Motivation: Merge subroutine naturally uncovers split inversions [as we'll see].

Augmenting our previous algorithm:-

    SortAndCount(array A, length n)
        if n = 1 return 0
        else
          B, x = SortAndCount(1st half of A, n/2)
          C, Y = SortAndCount(2nd half of A, n/2)
          D, Z = MergeAndCountSplitInv(A, n)
        return X + Y + Z

Looking at the merge subroutine again:-

    i = 1
    j = 1

    for k = 1 to n
      if B(i) < C(j)
        D(k) = B(i)
        i++
      else
        D(k) = C(j)
        j++

We use merge because, if there are any split inversions, then the __merge__ subroutine will
copy all elements from __B__ into __D__, before even touching __C__, because by definition
__B__ will contain elements all smaller than those in __C__. If it at any point __C__ contains
an element greater than __B__, this indicates that there is a split inversion.

Looking at an example:-

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-2.png" />

We see that when 2 is copied to the output, we have discovered the split inversions (3,2) and
(5,2).

Copies from the right array are interesting to us with respect to split inversions. Copies from
the left array aren't interesting.

### General Claim ###

Claim: The split inversions involving an element __Y__ of the 2nd array __C__ are precisely the
numbers left in the 1st array __B__ when __Y__ is copied to the output __D__.

Proof: Let __X__ be an element of the 1st array __B__.

1. If __x__ copied to output __D__ before __y__, then __X__<__y__ => no inversion involving
__x__ and __y__.

2. If __y__ copied to output __D__ before __x__, then __y__<__x__ => __x__ and __y__ are a
split inversion.

### Merge_and_Count_CountSplitInv ###

While merging the two sorted subarrays, keep running total of number of split inversions.

When elements of 2nd array __C__ gets copied to output __D__, increment total by number of
elements remaining in 1st array __B__.

Run time of subroutine: Merge is O(n) + running total O(n) = O(n).

The overall running time is O(n lg n), as we're using merge sort and this linear function, so
O(n lg n) dominates.

Strassen's Subcubic Matrix Multiplication Algorithm
---------------------------------------------------

Let's consider 3 matrices, X, Y and Z:-

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-3.png" />

We're not considering non-square matrices in this video.

Where:-

    [; Z_{ij} = (ith row of X) . (jth column of Y) ;]
    [; = \Sum_{k=1}^n x_{ik}.y_{kj} ;]

Note: Input size is = O(n^2).

The best we will be able to do with these matrices is an O(n^2) operation.

### Example (n=2) ###

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-4.png" />

Our naive solution has running time of:-

    [; \Theta(n^3) ;]

Because:-

    [; = \Sum_{k=1}^n x_{ik}.y_{kj} ;]

Has running time of:-

    [; \Theta(n) ;]

### Applying Divide and Conquer ###

Idea: Divide into quadrants (blocks in matrix terminology):-

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-5.png" />

### Recursive Algorithm #1 ###

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-6.png" />

Step 1: Recursively computer the 8 necessary products.

Step 2: Do the necessary additions (O(n^2) time).

Fact: Run time is:-

    [; \Theta(n^3) ;]

Which follows from the master method, and gives us nothing better than the naive approach
(yet).

### Strassen's Algorithm (1969) ###

Step 1: Recursively compute only 7 (cleverly chosen) products.

Step 2: Do the necessary (clever) additions + subtractions (still Theta(n^2) time).

Fact: Better than cubic time. See next lecture to see why (via master method).

Very surprising result. Shows how clever approach can make a huge difference.

### The Details ###

For matrices:-

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-7.png" />

The Seven Products:

    [; P_1 = A(F-H) ;]
    [; P_2 = (A+B)H ;]
    [; P_3 = (C+D)E ;]
    [; P_4 = D(G-E) ;]
    [; P_5 = (A+D)(E+H) ;]
    [; P_6 = (B-D)(G+H) ;]
    [; P_7 = (A-c)(E+F) ;]

Claim:

<img src="http://codegrunt.co.uk/images/algo/3-divide-and-conquer-algorithms-8.png" />

