Design and Analysis of Algorithms - Data Structures
===================================================

Data Structures: Overview
-------------------------

### Data Structures ###

__Point:__ Organise data so that it can be accessed quickly and usefully.

__Examples:__ Lists, stacks, queues, heaps, search trees, hash tables, bloom filters,
union-find, etc.

__Why so many?:__ Different data structures support different sets of operations => suitable
for different types of tasks.

__Rule of Thumb:__ Choose the 'minimal' data structure that supports all the operations that
you need.

### Taking It To the Next Level ###

Levels of Understanding:-

__Level 0:__ Total ignorance. Unaware that organising your data might improve the performance
of your software, etc. - "What's a data structure?"

__Level 1:__ Cocktail-part literacy - have a rough idea of how these things work, but a bit
shaky in implementing them.

__Level 2:__ Solid understanding - "this problem calls out for a heap"

__Level 3:__ Deep understanding - "I only use data structures that I wrote myself"

The aim is to take us to *level 2*.

Heaps - Operations and Applications
-----------------------------------

### Heap: Supported Operations ###

* A container for objects that have keys, e.g. employer records, network edges, events, etc.

* Two basic operations:-

__INSERT:__ Add a new object to a heap. O(log n) time. n = number of items in heap.

__EXTRACT-MIN:__ Remove an object in a heap with a minimum key value. [ties broken arbitrarily]
O(log n) time. n = number of items in heap.

Also: __Heapify__ - n batched inserts in O(n) time. Delete - O(log n) time.

It's almost fairly arbitrary that it's a minimum value we're retrieving, we can construct a
max-heap, or we could just negate the sign of keys.

### Application: Sorting ###

__Canonical use of heap:__ Fast way to do repeated minimum computations.

__Example:__ Selection sort - scan for the minimum and put in the first position, repeat for
n-1 elements, etc. Clearly `[; \Theta(n) ;]` linear scans, `[; \Theta(n^2) ;]` runtime on array
of length n.

Clearly this is a target for a heap data structure, as we are constantly looking for a minimum
value. So we can define __HeapSort__:-

1. Insert all __n__ array elements into a heap.

2. Extract-Min to pluck out elements in sorted order.

Running time = 2n heap operations = O(n log n) time.

* Optimal for a "comparison-based" sorting algorithm!

### Application: Event Manager ###

"Priority Queue" - synonymous to a heap.

__Example:__ Simulation (e.g. for a video game)

* Objects = event records [action/update to occur at given time in the future]

* Keys = time event scheduled to occur

* Extract-Min => yields the next scheduled event.

### Application: Median Maintenance ###

__I give you:__ A sequence x1,...,n of numbers, one-by-one.

__You tell me:__ At each time step i, the median of {x1,...,xi}.

__Constraint:__ Use O(log i) time at each step i.

__Solution:__ Maintain heaps `[; H_{LOW} ;]` : supports EXTRACT-MAX, `[; H_{HIGH} ;]` :
supports EXTRACT-MIN.

__Key Idea:__ Maintain invariant that i/2 smallest (largest) elements in `[; H_{LOW} ;]`
(`[; H_{HIGH} ;]`).

__YOU CHECK:__ 1. can maintain invariant with O(log i) work, 2. given invariant, can compute
median in O(log i) work

### Application: Speeding Up Dijkstra ###

#### Dijkstra's Shortest-Path Algorithm ####

* Naive implementation -> `[; \Theta(nm) ;]`

* With heaps -> run time = `[; O(m \log n) ;]`

Hash Tables - Operations and Applications
-----------------------------------------

* Vital data structure.

### Hash Table: Supported Operations ###

E.g. phonebook. If we tried to keep an array with indexes for all possible positions, we'd
require vast, vast amounts of memory (e.g. `[; 26^{30} ;]`).

__Purpose:__ Maintain a (possibly evolving) set of stuff (transactions, people + associated
data, IP addresses, etc.)

__Insert:__ Add new record

__Delete:__ Delete existing record

__Lookup:__ check for particular record

All of these using a key.

Sometimes referred to a 'dictionary'. Bad analogy, typically not kept in order. Better to use a
binary tree/heap for that kind of thing.

__AMAZING GUARANTEE:__ These operations run in O(1) time!*

* 1: Needs to be properly implemented, otherwise not guaranteed. 2: No worst-case
  guaranteed. For non-pathological data, you will get constant time performance.

### Application: De-Duplication ###

__Given:__ A "stream" of objects. E.g. Linear scan through a huge file/objects archiving in
real time.

__Goal:__ Remove duplicates (i.e. keep track of unique objects).

E.g. report unique visitors to websites/avoid duplicates in search results.

__Solution:__ When new object __x__ arrives - lookup __x__ in hash table __H__, if not found,
insert __x__ into __H__.

### Application: The 2-SUM Problem ###

__Input:__ Unsorted array of n integers. Target sum t.

__Goal:__ Determine whether or not there are two numbers __x__, __y__ in __A__ with __x + y = t__.

__Naive Solution:__ `[; \Theta(n^2) ;]` time via exhaustive search.

__Better:__ 1. sort A (`[; \Theta(n log n) ;]` time), 2. For each __x__ in __A__ look for __t -
x__ in __A__ via binary search.

__Amazing:__ 1. Insert elements of __A__ into hash table __H__. 2. For each __x__ in __A__ look
for __t-x__ in __H__.

### Further Immediate Applications ###

* Historical applications: Symbol tables in compilers

* Blocking network traffic

* Game tree exploration, e.g. chess engine :-) - use hash table to avoid exploring any
  configuration (e.g. arrangement of chess pieces) more than once.

* etc. etc.

Hash Tables- Implementation Details, Part I
-------------------------------------------

### High-Level Idea ###

__Setup:__ Universe __U__ (e.g. all IP addresses, all names, all chess board configurations,
etc.) [generally, REALLY BIG]

__Goal:__ Want to maintain evolving set __S__ which is a subset of
__U__. [generally, of reasonable size]

__Naive solutions:-__

1. Array-based solution [indexed by U] - O(1) operators, but `[; \Theta(|U|) ;]`
space. Basically infeasible for most problems.

2. List-based solution `[; \Theta(|S|) ;]` space, but `[; \Theta(|S|) ;]` lookup.

__Solution:-__

1. Pick n = # of "buckets" with n ~~ |S|,

2. Choose a hash function h:U->{0,1,2,...,n-1}

3. Use array __A__ of length __n__, store __x__ in __A[h(x)]__.

E.g. the birthday paradox. Despite it seeming unlikely, once you have 23 people in a room you
are 50% likely to get two with the same birthday. this is relevant to hash tables
w.r.t. collisions - so in this situation it means it takes 23 hash entries of keys as birthdays
before it's equally likely than not that we get a collision. So collisions are rather common.

### Resolving Collisions ###

__Collision:__ Distinct `[; x,y \in U ;]` such that `[; h(x) = h(y) ;]`

E.g.:-

<img src="http://codegrunt.co.uk/images/algo/12-data-structures-1.png" />

__Solution #1:__ (Separate) chaining.

* Keep linked list in each bucket.

* Given a key/object x, perform Insert/Delete/Lookup in the list in A[h(x)].

__Solution #2:__ Open addressing. (Only one object per bucket)

* Hash function now specifies probe *sequence*. h1(x), h2(x), Keep trying until find open slot.

* Examples: Linear probing (look consecutively) - i.e. add a factor each time. Double hashing -
  use 2 hash functions. 

* Uses less memory.

* Deletion more tricky.

Hash Tables- Implementation Details, Part II
--------------------------------------------

### What Makes a Good Hash Function? ###

* __Note:__ In hash table with chaining, Insertion is `[; \Theta(1) ;]` - inserting at the beginning
of a linked list is constant time, so it follows that insertion to the hash table will be too.

* O(list length) for lookup/insert/delete - the list length could be anywhere from m/n - equal
  length lists, where m is the number of objects, and n is the number of buckets, to m, where
  everything is put in the same list.

* __Point:__ Performance depends on the choice of hash function!

* Analogous situation with open addressing.

* Properties of a "good" hash function:-

1. Should lead to good performance => i.e. should "spread data out" - as evenly as possible
across buckets. __Gold standard:__ completely random hashing.

2. Should be easy to store/very fast to evaluate.

### Bad Hash Functions ###

* __Example:__ Keys = phone numbers (10-digits). `[; |u| = 10^{10} ;]`

* Choose `[; n = 10^3 ;]`

* *Terrible* hash function: h(x) = 1st 3 digits of x.

* *Mediocre* hash function: h(x) = last 3 digits of x.

* __Example:__ Keys = memory locations.

* Will be multiples of a power of 2.

* Bad hash function: h(x) = X mod 1000 (again `[; n = 10^3 ;]`).

* All odd buckets guaranteed to be empty.

### Quick-and-Dirty Hash Functions ###

We can see hashing in the following way:-

<img src="http://codegrunt.co.uk/images/algo/12-data-structures-2.png" />

How to choose n = # of buckets
------------------------------

1. Choose n to be a prime (within constant factor of # of objects in table).

2. Not too close to a power of 2

3. Not too close to a power of 10
