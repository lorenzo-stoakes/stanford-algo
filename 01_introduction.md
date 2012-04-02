Design and Analysis of Algorithms I - Introduction
==================================================

Why Study Algorithms?
---------------------

What is an algorithm?

An algorithm is essentially a well-defined set of rules/ recipe for solving a computational
problem.

E.g. you want to sort some numbers, find the shortest path in a network, etc. etc.

Why study them?

* Important for every other branch of computer science. You can't really do serious work in
  computing without them. This is why Stanford insist on the course for everyone no matter what
  they're doing.

* Plays a key role in modern technological innovation, e.g. google.

* Algorithms have been used as a 'lens' on other areas.

* They're challenging (i.e. good for the brain).

* Designing + implementation algorithms is just fun :-)

### Integer Multiplication ###

Input: 2 n-digit numbers.

Output: x * y.

We are interested in counting how many steps are required in performing the computation. We
count how many 'primitive operations' are required, which we define (for now) as:-

Primitive Operation: add/multiply 2 single-digit numbers.

### Grade-School Algorithm ###

Let's consider the approach taught to school kids:-

        5678
    x   1234
    --------
       22712
      17034-
     11356--
     5678---
    --------
     7006652

We end up with a grid of n x n numbers. So overall we're looking at O(n^2).

no. of operations overall is approximately: constant * n^2.

### The Algorithm Designer's Mantra ###

Can we do better?

Especially when we are faced with a naive/obvious solution to the problem.

Let's define:-
    x = 10^(n/2)a + b
and:-
    y = 10^(n/2)c + d

Where a, b, c, and d are n/2-digit numbers.

E.g.:- a = 56, b = 78, c = 12, d = 34.

Then:-

    x.y = (10^(n/2)a + b)(10^(n/2)c + d)
        = (10^n)ac + 10^(n/2)(ad + bc) + bd (*)

We can recurse over number of digits until a base case.

The speed/wisdom of this approach is not obvious. We will explore this in a later lecture.

### Karatsuba Multiplication ###

If we look at the equation we obtained previously:-

    x.y = (10^n)ac + 10^(n/2)(ad+bc) + bd

We notice there are 4 products, but in fact there really are only three (due to Gauss) - ac,
(ad + bc), bd.

We don't care about ad and bc separately, rather we care are the sum. This raises the question
- is there a way of calculating this result with only 3 calls instead of 4? And it turns out
there is. This is known as Karatsuba multiplication.

Steps:-

1. Recursively compute ac.
2. Recursively compute bd.
3. Recursively compute (a+b)(c+d) = ac + ad + bc + bd.

Gauss's trick:-

    (3) - (1) - (2)

Gives us:-

    = ad + bc

Upshot: Only need 3 recursive multiplications (and some additions).

The algorithm design space is incredibly rich. This is a great example - you wouldn't
immediately guess there was so much depth to it.

Q: Which is the fastest algorithm? Hard to tell intuitively.

About the Course
----------------

* Topics we're going to cover
* Skills to be acquired
* Kind of background that is expected
* Supportive materials
* Tools for self-assessment

### Course Topics ###

The idea is to provide basic introduction to each. There is a lot more that can be said about
all of these.

* Vocab. for design + analysis of algorithms

This is possibly one of the driest topics, but it's a pre-requisite for thinking about the
design of algorithms.

We'll start with Big-Oh notation. Conceptually, it's a modelling choice with which we measure
the granularity with which we measure the performance metric of an algorithm like the running
time.

It turns out that a sweet spot for considering algorithm design is to ignore constants and
lower-order terms and concentrate on how well algorithms scale with large input sizes.

Big-Oh is a way of 'mathematicise' this sweet spot.

* Divide and Conquer Algorithm Design Paradigm

There's no silver bullet in algorithm design. No single method which 'unlocks' all the
computational problems you're likely to face. There are however a few algorithm design
techniques, high-level approaches which can be used successfully across a range of different
problems.

In this course, we'll only have to time to consider one such paradigm, namely
divide-and-conquer algorithm design.

The idea is that we break a problem into smaller subproblems which we then solve recursively,
and then to somehow quickly combine the solutions to the subproblems into a solution which
solves the original problem.

E.g. We saw two d&c approaches in the previous video.

We'll consider:-

- Integer multiplication, sorting, matrix multiplication and closest pair.
- General analysis methods ("master method/theorem").

* Randomisation in algorithm design

We essentially 'flip coins' in this approach - i.e. if you were to run the solution multiple
times you would get different executions.

Totally un-intuitively, it turns out that allowing internal randomisation often leads to
simple, elegant and practical solutions to computation problems.

The canonical example is randomised quicksort, which we will look at in a few lectures.

We'll apply this technique to quicksort, primality testing, graph partitioning and hashing.

* Primitives for reasoning about graphs

One key skill to take away from the course is literacy with a number of computational
primitives operating on data which are so fast that they are, essentially, free. The time it
takes to invoke one of these computational primitives is barely more than the time you are
already taking to examine the input. You should be applying it in case it is
useful. E.g. sorting.

We'll consider data which is complex in the form of a graph, which consists not only of a set
of vertices, but also edges between pairs of vertices, this is a graph.

Graphs model (amongst other things), different types of networks. Even though these are more
complicated than, say, arrays, there are still a number of very fast primitive operations you
can apply to graphs.

In this class we will consider connectivity information, shortest paths, structure of
information and social networks.

* Use and implementation of data structures

Data structures are often a crucial ingredient in the design of fast algorithms. Data
structures are responsible for organising data in a such a way that suits fast queries.

Different data structures are useful for different queries.

E.g. heaps, balance binary search trees, hashing and some variants (e.g. bloom filters).

We'll consider two data structures in particular detail:-

* Balanced Binary Search Trees - which dynamically maintain an ordering on a set of elements
  while supporting a number of queries which run in time logarithmic with respect to the size
  of the set.

* Hash Set - Keep track of a dynamic set which supporting extremely fast lookup and insertion.

We'll talk about canonical uses of these data structures and also what's going on 'under the
hood' in such data structures.

### Topics in Sequel Course ###

These are topics which will be covered in Design and Analysis of Algorithms II.

* Greedy algorithm design paradigm

* Dynamic programming algorithm design paradigm

* NP-complete problems and what to do about them - cannot be solved computationally efficiently

* NP-complete: Fast heuristics with provable guarantees

* NP-complete: Fast exact algorithms for special cases

* NP-complete: Exact algorithms that beat brute-force search

Depending on demand there might be further courses.

### Skills I Will Learn ###

* Become a better programmer.

* Sharpen your mathematical and analytical skills - want to explain *why* we do things a
  certain way. Good algorithmic analysis typically requires non-trivial mathematics.

* Start 'thinking algorithmically', not just in computer science.

* Literacy with computer science's 'greatest hits'.

* Ace your technical interviews.

### Who Are You? ###

* It doesn't really matter. (It's a free course, after all).

* Ideally, you know some programming.

* You should be capable of translating high-level algos into working programs in *some*
  programming language, doesn't matter which.

* Some (perhaps rusty) mathematical experience. E.g. discrete maths, proofs by induction, etc.

* Excellent free reference: "Mathematics for Computer Science" by Eric Lehman and Tom
  Leighton. Google it.

* All annotated slides available from course site.

For external sources, google Dasgupta/Papadimitriou/Vazirani, Algorithms - available online.

### Assessment ###

* No grades per se (details on certificate TBA).

* Weekly homework - tests understanding, synchronise students for discussion, intellectual
  challenge.

* Occasionally propose harder "challenge problems".

Merge Sort: Motivation and Example
----------------------------------

### Why Study It? ###

* Merge sort is an old algorithm, but it is still used heavily in the real world. Good
  introduction to divide + conquer.

Generally speaking, merge sort is an improvement over selection, insertion and bubble sorts.

* One of the most transparent examples of the divide and conquer approach available.

* Calibrates your preparation. Can you handle the course?

* Motivates guiding principles for algo analysis - worst-case and asymptotic analysis.

* Recursive tree analysis generalises to "master method".

### The Sorting Problem ###

Input: array of n numbers, unsorted.
Output: Same numbers, sorted in increasing order.

For the purpose of this lecture, we assume the elements are distinct. Can look at ties as an
additional piece of work.

In a picture:-

        5,4,1,8,7,2,6,3
    5,4,1,8         7,2,6,3
        recursive calls
             merge
        1,2,3,4,5,6,7,8

Merge Sort: Pseudocode
----------------------

Let's look at the algorithm without the merging call for now. The steps are:-

* Recursively sort 1st half of input array
* Recursively sort 2nd half of input array
* Merge two sorted sublists into one

For now we're ignoring base cases.

Note that odd numbered input sets can divide either way, we don't care how.

Let's look at the pseudocode for the merge step:-

    C = output array [length=n]
    A = 1st sorted array [n/2]
    B = 2nd sorted array [n/2]

We'll define three counter variables - i, j and k. We use i and j to traverse A and B
respectively, and k to traverse C.

    i = 1
    j = 1

    for k = 1 to n
      if A(i) < B(j)
        C(k) = A(i)
        i++
      else
        C(k) = B(j)
        j++

Again we're ignoring end cases, i.e. when we reach the end of A and B.

### Merge Sort Running Time? ###

What is the running time of the merge sort algorithm?

We won't provide a really rigorous definition for 'running time' for the time being, we'll look
at that later.

Imagine we're running in a debugger and we're pressing enter to advance through the code, we're
counting the number of times we press enter before the program terminates.

Key Question: What is the running time of Merge Sort on an array of n numbers?

Running time is roughly equal to the number of lines of code executed.

Let's look at the running time of a single merge of two subarrays:-

    i = 1
    j = 1 } - 2 operations

    for k = 1 to n
      if A(i) < B(j) } 1 operation for both branches.

        C(k) = A(i)  } 1 operation
        i++          } 1 operation
      else
        C(k) = B(j)  } 1 operation
        j++          } 1 operation

      (implied k++)  } 1 operation

### Running Time of Merge ###

Upshot:

Running time of merge on array of m numbers is:-

    <= 4m + 2

We can be pedantic about how we count these values, but ultimately the differences don't matter
too much.

We can get even sloppier and just say:-

    <= 6m
    Since always the case that m >= 1

### Running Time of Merge Sort ###

We have a lot of subproblems to solve, recursively, but each subproblem is a lot smaller.

Our claim is that Merge Sort requires:-

    <= 6n lg n + 6n operations

To sort *n* numbers.

    lg n = # of times you divide by until you get down to 1.

Merge Sort: Analysis
--------------------

Looking at the running time analysis of the merge sort algorithm. Looking at the claim that the
divide and conquer approach gives you a better running time than simpler algorithms, e.g.
insertion sort/selection sort/bubble sort.

Looking at the claim we provided earlier:-

For every input array of n numbers, Merge Sort produces a sorted output array and uses at most
6n lg(n) + 6n operations.

How are we going to prove this claim (assuming n is a power of 2)? Using a recursion tree.
We'll draw the elements:-

<img src="http://codegrunt.co.uk/images/algo/1-introduction-1.png" />

The lowest level is lg(n), since we're dividing each node's element count by 2 on each level in
the tree, which is exactly the definition of lg(n) we gave earlier.

Give we start at level 0, this leaves us with a total of lg(n) + 1 levels to deal with.

At each level j=0,1,2,...,lg n there are 2^j subproblems, each of size n/2^j.

### Proof of Claim (assuming n = power of 2) ###

Merge sort is essentially very simple, there are two recursive lines of code, followed by a
call to the merge function.

We ignore the recursive calls for the time being, and look only at the merge call.

We know already that the merge call requires:-

    <= 6m

Lines of code.

At each level there are 2^j subproblems, each of size n/2^j. We simply multiply the number of
level-j subproblems vs the number of executions required for the subproblem size to get the
total run time at level j:-

<img src="http://codegrunt.co.uk/images/algo/1-introduction-2.png" />

Note that 2^j cancels out and it turns out that we still have an upper bound of:-

    <= 6n

This is because of a perfect equilibrium between two forces - the number of subproblems is
doubling, but the amount of work done is halving each level.

We can now get the total number of execution steps required for the algorithm by multiplying
the number of levels by the work required on each level, e.g.:-

    (lg(n) + 1) . 6n

So our upper bound is, as previously claimed:-

    6n . (lg(n) + 1)

Guding Principles for Analysis of Algorithms
--------------------------------------------

Let's look at 3 biases/assumptions we made when we performed the analysis of merge sort. These
are assumptions we will be making throughout the rest of the course.

### Guiding Principle #1 ###

We look at the *worst-case* - we are performing "worst-case analysis".

Our running time bound holds for *every* input of length n.

As opposed to alternative analysis approaches:-

* Average-Case Analysis - We look at expected input and base our analysis on this, e.g.
considering that each possible input is equally likely.

* Benchmarks - We have some convoluted input sets which we consider represent real-world input.

For either of these alternatives we need domain knowledge of the problem space to be able to
choose appropriate inputs.

On the contrary, worst-case analysis requires no prior knowledge. This is particularly
appropriate for 'general-purpose' routines.

Worst-case analysis is typically more mathematically tractable than these alternatives.

### Guiding Principle #2 ###

We won't pay much attention to constant factors, or lower-order terms.

We saw this in the merge sort analysis - we moved from 4n + 2 to 6n.

Justifications
--------------

1. Considerably easier, mathematically!

2. Constants depend on architecture/compiler/programmer in any case.

3. Lose very little predictive power. We will see this.

### Guiding Principle #3 ###

We're performing *asymptotic analysis*. We're considering large input sizes - we're focusing on
running time for *large* input sizes n.

E.g.:-

    6n lg n + 6n

Is better than:-

    0.5n^2

This is true iff n is large.

Justifications
--------------

Only big problems are interesting. E.g. sorting 100 numbers will be quick no matter of the
algorithm choice.

A corollary of Moore's law is that as computers get more powerful, we will run algorithms
against ever larger datasets, and the gulf between an n^2 and an n.lg(n) algorithm will only
get larger.

To drive the point home, consider the following graph comparing the run times of merge sort and
insertion sort:-

<img src="http://codegrunt.co.uk/images/algo/1-introduction-3.png" />

We see that insertion sort is faster than merge sort *up to a point*, roughly n=90 here.

This is even clearer if we increase the input size:-

<img src="http://codegrunt.co.uk/images/algo/1-introduction-4.png" />

All this doesn't mean you get to completely ignore constant factors. Often modern sorting
algorithms switch between algorithms at small sizes to handle these cases.

### What Is a "Fast" Algorithm? ###

This course: Adopt these three biases as guiding principles.

A fast algorithm is one where the worst-case running time grows slowly with the input size.

We want a sweet spot - between mathematical tractability and predictive power. Worst-case
asymptotic analysis fits this role nicely.

Our Holy grail is running time growing linearly with input size, or at least as close to it as
we can manage.
