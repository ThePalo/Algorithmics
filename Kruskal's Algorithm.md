<h1> Kruskal's Algorithm </h1>

Graph for this exercice:
> - Nodes: fixed population of n size
> - Ages: this graph only can evolve with addition of adges. Variable size m

Problem to resolve: Finding minimum spanning tree.
> Aproach: Starts without any edges at all and builds a spanning tree by successively inserting edges from E in order of increasing cost. As we move through the edges in this order, we insert each edge *e* as long as it does not create a cycle when added to the edges we’ve already inserted. If, on the other hand, inserting *e* would result in a cycle, then we simply discardeand continue.

Considerations to resolve this problem:
> - When an edge is added to the graph, we don’t want to have to recompute the connected components from scratch
> - Develop a data structure [Union-Find structure] -> representation of the components in a way that supports rapid searching and updating.

#### Union-Find
Maintain disjoint sets (such as the cc of a graph). Each set is a Graph cc.
- **MakeUnionFind(S)**: for a set S will return a Union-Find data structure on set S where all elements are in separate sets
    - Time: O(n)
- **Find(u)**: return name of the set containing u.
    - Time: O(log n) or O(1)
- **Union(A,B)**: merge sets A and B to a single set
    - Time: O(log n)

#### Union-Find data strucutre (better way)
- Each node v ∈ S will be contained in a record with an associated pointer to the name of the set that contains v.
- Use the elements of the set S as possible set names, naming each set after one of its elements.
- **MakeUnionFind(S)**: initialize a record for each element v ∈ S with a pointer that points to itself (or is defined as a null pointer), to indicate that v is in its own set.
- **Union(A,B)**: sets named by node v ∈ A and node u ∈ B. And the result is named by v ∈ A. To indicate that we took the union of the two sets, and that the name of the union set is v:
    1. Update u’s pointer to point to v
    2. Not update the pointers at the other nodes of set B.
    
![Union example](https://github.com/MrRobb/SO-FIB/blob/master/Utilidades/img%20resum/img4.png)

> **Important** Keep the name of the larger set as the name of the union
> - To do it efficiently: additional field with the nodes --> the size of the corresponding set.
- **Find(u)**: why now its time is O(log n)?
> The time to evaluate Find(v) for a node v is the number of times the set containing node v changes its name during the process. By the convention that the union keeps the name of the larger set, it follows that every time the name of the set containing node v changes, the size of this set at least doubles. Since the set containing v starts at size 1 and is never larger than n, its size can double at most log2 n times, and so there can be at most log2 n name changes.

#### Union-Find data strucutre (future improvments)
Main idea of what we can do in order to increase the optimitzation:

![Union example](https://github.com/MrRobb/SO-FIB/blob/master/Utilidades/img%20resum/img4.png)

- Find(u): after finding that u ∈ A (and A cc is named by node x) we can point all the nodes to x as in the image. Cost can be higger, as since after finding the name x of the set containing v, we have to go back through the same path of pointers from v to x, and reset each of these pointers to point to x directly. The real gain from compression is in making subsequent calls to Find cheaper; bounding the total time for a sequence of n Find operations, rather than the worst-case time for any one of them.

#### Implementing Kruskal’s Algorithm
Use the Union-Find data structure to implement Kruskal’s Algorithm.
1. Sort the edges by cost. Time O(m log m), but as m >= n^2, time is O(m log n)
2. Use the Union-Find data structure to maintain the connected components of (V, T) as edges are added.
    - Edge e = (w,v) added to graph
        - if (Find(u) == Find(v)) //u and v are from the same cc
        - else: merge cc of v and cc of v: Union(Find(u), Find(v)) //now we have one cc
> Cost:
> - 2m Finds at most
> - n-1 Unions at most
> - Sort adges: O(m log n)
> Total cost of Kruskal's Algorithm: O(m log n)
