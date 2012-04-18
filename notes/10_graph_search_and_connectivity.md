Design and Analysis of Algorithms - Graph Search and Connectivity
=================================================================

Graph Search - Overview
-----------------------

* Searching a graph is a fundamental problem. Related to that is finding paths through graphs.

### A Few Motivations ###

1. Physical network - making sure it is fully-connected, e.g. you can get from one point to
   another point. E.g. telephone networks, road networks, undirected graph of movie network
   where nodes are actors and the edges are films in which they appeared together , e.g. Kevin
   Bacon.

2. Shortest paths - e.g. driving directions.

3. Paths are not just physical things, more abstractly they are like a like a sequence of
   decisions taking you from an initial state to a final state. Useful for formulating a plan
   from an initial state to a goal state. E.g. solving a Sudoku puzzle, a directed graph, where
   nodes are partially completed puzzle states, and arcs represent filling in one squares.

4. Computing the "pieces" (or "components") of a graph, e.g. clustering heuristic in undirected
   graphs, more subtle in directed graphs.

Many algorithms for solving information relating to graphs are very cheap, e.g. linear, lots of
'for-free' primitives - processing steps, routines can be run without even thinking about it.

### Generic Graph Search ###

__Goals:__

1. Start with a given start vertex find everything findable from it.

E.g.:-

In an undirected graph, we can find the highlighted nodes starting from __S__:-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-1.png" />

And in the directed case:-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-2.png" />

We can reach nodes in the green area, not the blue node.

2. Don't explore anything twice (goal is for time complexity of O(m+n) - i.e. linear in the
size of the graph).

### Generic Algorithm (Given Graph G, Vertex S) ###

* Initially __s__ explored, all other vertices unexplored.

* Where possible:-

    1. Choose an edge (u, v) with u explored and v unexplored (if none, halt)

    2. Mark v explored.


Wherever you start the algorithm, you find everything findable and don't explore any node
twice.

The nodes we mark explored are, by definition, are the nodes we can find from the start node.

More formally:-

__Claim:__ At the end of the algorithm, __v__ explored <=> __G__ has a path from __s__ to
__v__, where __G__ is undirected or directed.

__Proof:__ (=>) easy induction of number of iterations (left as exercise to the reader.)

(<=) By contradiction. Suppose __G__ has a path __P__ from __s__ to __v__:

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-3.png" />

Consider that there exists an edge __(u, w)__ with __u__ explored and __w__ unexplored, the
algorithm will consume it. How can we possibly have an edge __(u, x)__ which remains
unexplored? This isn't possible, as the algorithm doesn't stop until it has exhaustively
'conquered all edges in this graph if it has terminated. So for this to be the case, the
algorithm wouldn't have terminated, so this is a contradiction. QED!

### Breadth-First-Search (BFS) vs. Depth-First-Search (DFS) ###

__Note:__ How to choose among the possibly many "frontier" edges?

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-4.png" />

Note that in directed graphs, we need to consider cases where the tail of the edge is in the
explored side, and the head in the unexplored side. In undirected graphs, this is clearly not a
consideration.

What is our strategy for picking the next unexplored node?

### Breadth-First-Search (BFS) ###

* Explore nodes in "layers", e.g. layer 0 contains __S__, the neighbours of __S__ are in layer
  1, the neighbours of layer 1 which are not in layer 0 or 1 are layer 2, etc. I.e. layer
  __i+1__ contains the nodes next to layer __i__ which we haven't seen yet.

* There is a close correspondence between these layers and shortest paths - if a node is in
  layer __i__, then it takes __i__ hops to get from the __S__ to this node.

* Can compute connected components of an undirected graph using this (also other strategies).

* We can get O(m+n) time with good constants using a queue (FIFO).

### Depth-First Search (DFS) ###

* O(m+n) time with good constants, using a stack (LIFO), which we can implement simply via
  recursion, the call stack taking care of this.

* Explore aggressively like a maze, backtrack only when necessary.

* E.g. Compute topological ordering of directed acyclic graph.

* Compute connected components in directed graphs.

Breadth-First Search - The Basics
---------------------------------

### Overview and Example ###

* Explore nodes in "layers":-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-5.png" />

* Can compute shortest paths.

* Connected components of undirected graphs

### Run time ###

Want O(m+n) - linear time!

### The Code ###

    BFS(graph G, start vertex s):
        [all nodes initially unexplored]
        * Mark s as explored.
        * Let Q = queue data structure (FIFO), initialised with S.
        * While Q != 0 (O(1)):-
          * Remove the first node of Q (O(1)), call it v.
          * For each edge (v, w):
            * If w unexplored:-
                * Mark w as explored:-
                * Add w to Q (at the end).

In the first iteration we have:-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-6.png" />

Where we (though it's not required that we go in this order) look at __a__, note that it is
unexplored, and add it to the end of our queue, then do the same with __b__, we end up with the
queue as shown above.

__Note:__ The start of queue is on the left, the end of queue is on the right.

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-7.png" />

Next, we remove __a__ from the start of the queue, and consider whether __s__ is
unexplored. Clearly it has been explored, so we don't do anything with it. Next, we look at
__c__ and note that it *is* unexplored, and add it to the end of the queue.

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-8.png" />

Next, we remove __b__ from the start of the queue, consider __s__, note that it is explored and
thus do nothing with it, then consider __c__, again it is explored (only just noted as such),
so do nothing with it, then move to __d__, note that this *is* unexplored and thus add it to
the end of the queue.

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-9.png" />

Next, we remove __c__ from the start of the queue, we consider __b__, __a__, and __d__, noting
that __b__ and __a__ have already been seen, but not __d__, so we add this to the end of the
queue.

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-10.png" />

Next, we remove __d__ from the start of the queue, we consider __b__, __c__, and __e__, noting
that __b__ and __c__ have already been seen, but not __e__, so we add this to the end of the
queue.

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-11.png" />

Next, we remove __e__ from the start of the queue, we consider __c__ and __d__, noting that
we've already seen them, then loop round to the start of the while loop again, determine that
the queue is empty and exit.

Note that the numbering (i.e. the order in which the nodes were traversed) are equivalent to
the previously-discussed "layers" of nodes:-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-12.png" />

By using a FIFO data structure, i.e. a queue, we do end up processing the nodes in the order of
the layers.

We can see this finds everything that is findable, without redundancy, as we required.

### Basic BFS Properties ###

__Claim #1:__ At the end of BFS, __v__ explored <=> __G__ has a path from __s__ to __v__.

__Reason:__ Special case of the generic algorithm.

__Claim #2:__ Running time of main while loop = O(n\_s + m\_s), where:

    n_s = # of nodes reachable from s
    m_s = # of edges reachable from s

__Reason:__ Inspection of code.

Tallying up the time taken in the main while loop:-

        * While Q != 0 (O(1)):-
          * Remove the first node of Q (O(1)), call it v.  } O(n_s)
          * For each edge (v, w):                          } O(m_s)
            * If w unexplored:-                            }
                * Mark w as explored:-                     } O(1)
                * Add w to Q (at the end).                 }

Linear-time implementation, useful as breadth-first search generally, but also useful as a
workhorse for other tasks.

Breadth-First Search and Shortest Paths
---------------------------------------

Let's consider the movie graph once again, e.g.:- Richard Bacon -> John Ham.

### Application: Shortest Paths ###

__Goal:__ Compute __dist(v)__, the fewest # of edges on a path from __s__ to __v__.

__Extra Code:__

* Initialise dist(v) = { 0 if v = s, +inf if v != s }

* When considering edge (v, w):

    If w unexplored, then set dist(w) = dist(v) + 1.

E.g.:-

<img src="http://codegrunt.co.uk/images/algo/10-graph-search-and-connectivity-13.png" />

__Claim:__ At termination, __dist(v)__ = __i__ <=> __v__ in __i__th layer (i.e., <=> shortest
__s-v__ path has __i__ edges).

__Proof Idea:__ Every layer-i node __w__ is added to __Q__ by a layer-(__i__-1) node __v__ via
the edge __(v,w)__.

This is really a special property of breadth-first search, you don't get this from depth-first
search. This is as opposed to undirected connectivity (coming up) which does not rely on the
breadth-first search strategy.

Breadth-First Search and Undirected Connectivity
------------------------------------------------



Depth-First Search - The Basics
-------------------------------

Topological Sort
----------------

Computing Strong Components - The Algorithm
-------------------------------------------



Computing Strong Components - The Analysis
------------------------------------------

