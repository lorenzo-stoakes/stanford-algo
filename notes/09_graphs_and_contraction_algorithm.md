Design and Analysis of Algorithms I - Graphs and Contraction Algorithm
======================================================================

Graphs and Minimum Cuts
-----------------------

Relatively recent algorithm - "only" 20 years ago!

### Graphs ###

Represents pairwise relationships amongst a set of objects.

Two ingredients:-

* Vertices (a.k.a. nodes) (V)

* Edges (E) = pairs of vertices - can be undirected [unordered pair] or directed [orded pair]
  (head/tail pairs) a.k.a. arcs.

Better with a picture:-

<img src="http://codegrunt.co.uk/images/algo/9-graphs-and-contraction-algorithm-1.png">

The 'head' vertex of the ordered pairs is the vertex being pointed to, the vertex we're
pointing from is the 'tail' vertex of the edge.

Examples: roads networks, the web, social networks, precedence constraints, etc.

### Cuts of Graphs ###

__Definition:__ A *cut* of a graph (V, E) is a partition of V into two non-empty sets A and B.

we can divide edges into three types:-

#### Undirected ####

<img src="http://codegrunt.co.uk/images/algo/9-graphs-and-contraction-algorithm-2.png">

* Edges with end points in section A.
* Edges with end points in section B.
* Edges with end points in both A and B. 

#### Directed ####

<img src="http://codegrunt.co.uk/images/algo/9-graphs-and-contraction-algorithm-3.png">

* Edges with end points in section A.
* Edges with end points in section B.
* Edges with tail in A and head in B.
* Edges with tail in B and head in A.

__Definition:__ The *crossing edges* of a cut (A, B) are those with:-

* One endpoint in each of (A, B) [undirected]
* Tail in A, head in B [directed].

We have a binary choice for each vertex, so (if we ignore the empty set rule), we have 2^n
possible cuts for a graph with n vertices. Strictly speaking, if we do incorporate the empty
set rule, then there are 2^n-2 possible cuts, however 2^n is close enough.

### The Minimum Cut Problem ###

__Input:__ An undirected graph G=(V,E). [Parallel edges allowed].

Parallel edges are multiple edges which have the same source/destination. More meaningful to
naturally include parallel edges in the min cut problem than others.

__Goal:__ Compute a cut with fewest number of crossing edges (a *min cut*).

### A Few Applications ###

* Can identify weaknesses/bottlenecks in a network.

* Community detection in social networks. Can be a good first heuristic for highly-connected
  subgraphs with far less connection to the 'outside' world, that barrier to the outside world
  would be the crossing point.

* Image segmentation, with input = graph of pixels, use edge weights
  [(u,v) has large weight, expect u,v to come from the same object]. Hope that repeated min
  cuts identifies the primary objects in the picture.
  
Graph Representations
---------------------

* What do we mean by the size of a graph?

* For an array we have one parameter - the length of the array. For a graph we have to contend
  with the number of vertices and the number of edges, traditionally n vertices and m edges.

Considering an undirected graph with n vertices, no parallel edges and is connected, the number
of edges is:-

    [; n - 1 \leq m \leq n(n-1)/2 ;]

### Sparse vs. Dense Graphs ###

Let n = # of vertices, m = # of edges.

In most (but not all) applications, m is:-

    [; \Omega(n) ;]

And:-

    [; O(n^2) ;]

* In a 'sparse graph', m is O(n) or close to it.

* In a 'dense graph', m is closer to O(n^2).

Note, this terminology is quite loose in practice.

### The Adjacency Matrix ###

Represent G by a nxn O-1 matrix A where:-

    [; A_{ij} <=> G ;] has an i-j edge i--j.

#### Variants ####

* A_ij = # of i-j edges (if parallel edges).

* A_ij = weight of i-j edges (if any).

* A_ij = +1 if i->j, -1 if j->i.

An adjacency matrix requires [; \Theta(n^2) ;] space.

This is very wasteful if the graph is sparse.

### Adjacency Lists ###

#### Ingredients ####

* Array (or list) of vertices. Space is [; \Theta(n) ;].

* Array (or list) of edges. Space is [; \Theta(m) ;].

* Each edge points to its endpoints. Space is [; \Theta(m) ;].

* Each vertex points to edges incident on it. Space is [; \Theta(m) ;] as there is a one-to-one
  correspondence between this category and the edge -> edgepoints category.

This requires [; \Theta(m+n) ;] space, or [; \Theta(max{m,n}) ;] which is only a constant away
from the former.

__Question:__ Which choice is better - adjacency matrix/list?

__Answer:__ Depends on graph density and operations needed.

This course focuses on adjacency lists. Great for graph searches. Also far more efficient (and
actually possible) for huge graphes, clearly, which are the more interesting ones
e.g. web/social networks. Consider web which is say 10^10 vertices.

Random Contraction Algorithms
-----------------------------

* For solving the min cut problem.

Let's review that problem:-

__Input:__ An undirected graph G=(V,E). [Parallel edges allowed].

Parallel edges are multiple edges which have the same source/destination. More meaningful to
naturally include parallel edges in the min cut problem than others.

__Goal:__ Compute a cut with fewest number of crossing edges (a *min cut*).

### Random Contraction Algorithm ###

Due to Karger, early 90's.

    While there are more than 2 vertices:
      * Pick a remaining edge (u, v) uniformly at random.
      * Merge (or "contract") u and v into a single vertex.
      * Remove self-loops.
    Return cut represented by final 2 vertices.

### Example ###

<img src="http://codegrunt.co.uk/images/algo/9-graphs-and-contraction-algorithm-4.png">

### Example (con'd) ###

<img src="http://codegrunt.co.uk/images/algo/9-graphs-and-contraction-algorithm-5.png">

Randomised algorithms can behave differently on the same input.

Clearly doesn't always find the min cut.

Key Question - What is the probability of success? We shall see in the next section via
conditional probability.

Analysis of Contraction Algorithms
----------------------------------
