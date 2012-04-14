Design and Analysis of Algorithms I - Quicksort - Algorithm
===========================================================

QuickSort: Overview
-------------------

### QuickSort ###

* Definitely a 'greatest hit' algorithm.

* Prevalent in practice.

* Beautiful analysis.

* O(n log n) time 'on average', works in place, i.e. minimal extra memory
  needed. We'll see what 'on average' means later.

* Optional lecture notes available on website.

### The Sorting Problem ###

__Input:__ Array of n numbers in arbitrary order.

E.g.:- 3|8|2|5|1|4|7|6

__Output:__ Same numbers, sorted in increasing order.

E.g.:- 1|2|3|4|5|6|7|8

__Assume:__ All array entries distinct.
__Exercise:__ Extend to deal with duplicates.

### Partitioning Around a Pivot ###

__Key Idea:__ Partition array around a *pivot* element.

1. Pick one element in your array to act as a pivot.

To start with, let's arbitrarily choose the first element as the pivot.

2. Rearrange array so that left of pivot is less than the pivot, and the right of the pivot is
greater than the pivot.

E.g.:-

    2|1|_3_|6|7|4|5|8

Note that we don't sort the subarrays. We essentially 'bucket' the numbers into two sets.

__Note:__ Puts pivot in its 'rightful position'.

### Two Cool Facts About Partition ###

1. Can be done in linear (O(n)) time, no extra memory required.

2. Reduces problem size. Once we've partitioned, we need only sort the elements to the left of
the pivot, and the elements to the right of the pivot.

### QuickSort: High-Level Description ###

Discovered by Tony Hoore circa 1961.

    QuickSort(array A, length n):
        if n = 1 return
        p = Choose pivot(A,n)
        Partition A around p.
        Recursively sort 1st part.
        Recursively sort 2nd part.

Note there is no 'combine' step at all.

Partitioning Around a Pivot
---------------------------

### The Easy Way Out ###

If we didn't restrict to using an in-place algorithm, i.e. using constant memory, then the
solution would be easy.

We can do this by pre-allocated an extra array of length n, then copying elements greater than
the pivot to the right of the array moving to the left, and elements less than the array on the
left moving to the right.

### In-Place Implementation ###

__Assume:__ Pivot = 1st element of array.

(We can swap the pivot with the 1st element as a preprocessing step if this is not the case).

__High-Level Idea:__ 

<img src="http://codegrunt.co.uk/images/algo/5-quicksort-algorithm-1.png" />

* Single scan through array.

* Invariant: Everything looked at so far is partitioned.

We'll look at an example to start with.

### Partition Example ###

* We need to keep track of two boundaries - the split between things we've seen and not seen
  (we'll call this __j__), and the split between the two partitions (we'll call this __i__):-

<img src="http://codegrunt.co.uk/images/algo/5-quicksort-algorithm-2.png" />
<img src="http://codegrunt.co.uk/images/algo/5-quicksort-algorithm-3.png" />

### Pseudocode for Partition ###

    Partition(A, l, r) [ input = A[l...r] ]
        p := A[l]
        i := l+1
        for j = l+1 to r
            if A[j] < p
                Swap A[j] and A[i]
                i := i+1
        Swap A[l] and A[i-1]
