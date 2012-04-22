Design and Analysis of Algorithms - Dijkstra's Shortest Path Algorithm
======================================================================

Dijkstra's Shortest Path Algorithm
----------------------------------

* Another one of computer science's greatest hits.

### Single-Source Shortest Paths ###

__Input:__ Directed graph G=(V,E). (m=|E|,n=|V|)

* Each edge has non-negative length `[; l_e ;]`.

* Source vertex `s`.

__Output:__ For each `[; v \in V ;]`, compute `L(v):=` length of a shortest `s-v` path in `G`.

* The length of the path is the sum of the lengths of the edges along the path.

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-1.png" />

__Assumptions:__

1. [for convenience] `[; \forall v \in V, \exists ;]` an `s-v` path.

2. [important] No negative edge lengths, e.g. `[; l_e \geq 0 \forall e \exists E ;]`.

### Why Another Shortest-Path Algorithm? ###

__Question:__ Doesn't BFS already compute shortest paths in linear time?

__Answer:__ Yes, __IF__ `[; l_e=1 ;]` for every edge `e`.

__Question:__ Why not just replace each edge `e` by directed path of `[; l_e ;]` unit length
edges?

__Answer:__ Blows up graph too much.

__Solution:__ Dijkstra's shortest path algorithm.

### Dijkstra's Algorithm ###

* An example of the interconnectedness of data structures and algorithms.

* Very much related to breadth first search, and in fact if all the edge lengths are one, it
  effectively *becomes* breadth first search.

* Pseudocode:-

E.g.:-

    Initialise:

    - X    = {s}   [Vertices processed so far]
    - A[s] = 0     [Computed shortest path distances]
    - B[s] = empty [Computed shortest paths] - not necessary for an actual impl. useful for study.

    Main Loop:
    
    - while X != V: - Need to grow X by one node

    Main Loop cm'd:

    - Among all edges (v, w) in E with v in X, w not in X, pick one that minimises
      A[v] + l_{vw} - DIJKSTRA'S GREEDY CRITERION - call it (v*,w*).

    - Add w* to X

    - Set A[w*] := A[v*]+l_{v*w*}

    - Set B[w*] := B[v*] + (v*,w*)

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-2.png" />

Dijkstra's Algorithm: Examples
------------------------------

### Example ###

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-3.png" />

We make our first choice based on `[; A[v] + l_{vw} ;]`. Here we can see that (s, v) has cost
1, and (s, w) has cost 4, so we of course choose (s, v):-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-4.png" />

We consider (s, w), (v, w) and (v, t). (s, w) has cost 0 + 4 = 4, (v, w) has cost 1 + 2 = 3,
and (v, t) has cost 1 + 6 = 7, so we choose (v, w):-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-5.png" />

Now we consider (v, t) or (w, t). The score for (v, t) is still 1+6=7, and the score for (w, t)
is 3+3=6:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-6.png" />

We can't just take a single positive example as evidence that this algorithm works well for all
graphs.

### Non-Example ###

Let's consider an example where Dijkstra's algorithm breaks down, e.g. where we have negative
edge lengths.

__Question:__ Why not reduce computing shortest paths with negative edge lengths to the same
problem with nonnegative edge lengths? (by adding large constant to edge lengths)

__Problem:__ Doesn't preserve shortest paths! E.g. if you have a number of edges to pass
through, then the sum of these paths will go up by a multiple of the large number you're
adding.

E.g.:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-7.png" />

Here the shortest path is s->v->t, at a cost of -4.

Now after adding the largest negative number to all edges we end up with:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-8.png" />

From this we can see that now the shortest path has flipped, from s->v->t which now has cost 6,
to s->t which has cost 3.

__Also__: Dijkstra's algorithm is incorrect on this graph! This is because Dijkstra will choose
__(s, t)__ as its first addition to __X__, which means it has decided upon the shortest path
from __s__ to __t__, wrongly, as (s, t). E.g.:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-9.png" />

Dijkstra's Algorithm: Implementation and Running Time
-----------------------------------------------------

How do we implement Dijkstra? To start with, let's review the algorithm:-

### Single-Source Shortest Paths ###

__Input:__ Directed graph G=(V,E). (m=|E|,n=|V|)

* Each edge has non-negative length `[; l_e ;]`.

* Source vertex `s`.

__Output:__ For each `[; v \in V ;]`, compute `L(v):=` length of a shortest `s-v` path in `G`.

* The length of the path is the sum of the lengths of the edges along the path.

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-1.png" />

__Assumptions:__

1. [for convenience] `[; \forall v \in V, \exists ;]` an `s-v` path.

2. [important] No negative edge lengths, e.g. `[; l_e \geq 0 \forall e \exists E ;]`.

### Pseudocode ###

And the pseudocode for the algorithm:-

    Initialise:

    - X    = {s}   [Vertices processed so far]
    - A[s] = 0     [Computed shortest path distances]

    Main Loop:
    
    - while X != V: - Need to grow X by one node

    Main Loop cm'd:

    - Among all edges (v, w) in E with v in X, w not in X, pick one that minimises
      A[v] + l_{vw} - DIJKSTRA'S GREEDY CRITERION - call it (v*,w*).

    - Add w* to X

    - Set A[w*] := A[v*]+l_{v*w*}

* When we actually implement Dijkstra, we don't need to keep track of __B__, it's just there to
  help us understand the algorithm, so that has now been elided.

* The running time of Dijkstra implemented naively in the way described above, is
  `[; \Theta(mn) ;]`, because it iterates over every edge each time we add a node, looking for
  edges with their tail in __X__ and head in __V-X__. Keep in mind there is `[; \Theta(1) ;]`
  work done per edge.

* But can we do better? The answer is yes, via the use of a *heap* data structure.

### Heap Operations ###

* This is the first time that we're going to use a data structure to speed up an algorithm.

* What was the clue which indicated that this might be a good idea? It's that we're performing
  an exhaustive linear search on all the edges each time we run through the main loop.

__Recall:__ The raison d'etre of heap = perform insert, extract-min in O(log n) time.

* Conceptually, a perfectly balanced binary tree. This means the height is lg n, hence
  operations are run in O(log n) time.

* Hep property: At every node, key <= children's keys.

* Extract-min by swapping up last leaf, bubbling down.

* Insert via bubbling up.

__Also:__ Will need ability to delete from middle of heap (bubble up or down as needed).

### Two Invariants ###

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-10.png" />

__invariant #1:__ Elements in heap = vertices of __v-x__.

__invariant #2:__ For __v__ not in __X__, `Key[v]` = smallest Dijkstra greedy score of an edge
`[; (u, v) \in E ;]` with `[; u \in X ;]` or +inf if no such edges exist. E.g.:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-11.png" />

__Point:__ By invariants, Extract-Min yields correct vertex __w*__ to add to __X__ next.

(and we set `A[w*]` to `key[w*]`).

### Maintaining the Invariants ###

__To Maintain Invariant #2:__ [that, `[; \forall v \not \in X ;]`, Key[v] = smallest Dijkstra
greedy score of edge (u, v) with `[; u \in X ;]`]

* Tricky when adding new vertices to __X__, as they introduce new edges crossing the frontier
  from __X__ into __V-X__. This is because vertices which have edge heads coming into them from
  the newly added vertex, but also had heads coming into them from __X__ from other vertexes
  previously, might now have their Dijkstra greedy score reduced, requiring us to need to
  update keys in the heap. E.g.:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-12.png" />

* The keys we need to update here are the keys for the vertexes which have heads coming into
  them from edges coming from __w__.

When __w__ extracted from heap (i.e. added to __X__):-

    - For each edge [; (w, v) \in E ;]:
      - if [; v \in V - x ;] (i.e. in heap):
        - Delete v from heap
        - Recompute key[v] = [; min(Key[v], A[w] + l_{wv}) ;]
        - Re-insert v into heap (in O(lg n) time).

### Running Time Analysis ###

* Dominated by heap operations.

* (n-1) Extract mins.

* Each edge (v, w) triggers at most one Delete/Insert combo (if v added to X first). E.g.:-

<img src="http://codegrunt.co.uk/images/algo/11-dijkstras-shortest-path-algorithm-13.png" />

* __So:__ # of heap operations is O(n + m) = O(m) - since graph 'weakly connected'.

* __So:__ Running time = O(m log n), with good constants.
