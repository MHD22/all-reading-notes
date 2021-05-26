##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 34 - Graphs

<hr>

# Graphs:

A graph is a non-linear data structure that can be looked at as a collection of `vertices` (or `nodes`) potentially connected by line segments named `edges`.

Here is some common terminology used when working with Graphs:

1. `Vertex` - A vertex, also called a “node”, is a data object that can have zero or more adjacent vertices.
2. `Edge` - An edge is a connection between two nodes.
3. `Neighbor` - The neighbors of a node are its adjacent nodes, i.e., are connected via an edge.
4. `Degree` - The degree of a vertex is the number of edges connected to that vertex.

## Directed vs Undirected

### Undirected Graphs

An`Undirected Graph` is a graph where each edge is undirected or bi-directional. This means that the`undirected graph` does not move in any direction.

### Directed Graphs (Digraph)

A Directed Graph also called a `Digraph` is a graph where every edge is directed.

Unlike an `undirected graph`, a `Digraph` has direction. Each node is directed at another node with a specific requirement of what node should be referenced next.


## Complete vs Connected vs Disconnected:

* There are many different types of graphs. This depends on how connected the graphs are to other node/vertices.

* The three different types are completed, connected, and disconnected.

### Complete Graphs

A complete graph is when all nodes are connected to all other nodes. 

### Connected

A connected graph is graph that has all of vertices/nodes have at least one edge.

* A `Tree` is a form of a connected graph.

### Disconnected

A disconnected graph is a graph where some vertices may not have edges.

* It is complelty possible to have standalone nodes or edges (also known as islands) in a graph data structure.

## Acyclic vs Cyclic:

In addition to undirected and directed graphs, we also have acyclic and cyclic graphs.

### Acyclic Graph

An acyclic graph is a directed graph without cycles.

A cycle is when a node can be traversed through and potentially end up back at itself.

* A directed acyclic graph is also called a `DAG`. This can also be represented as what we know as a `tree`.

### Cyclic Graphs

A Cyclic graph is a graph that has cycles.

A cycle is defined as a path of a positive length that starts and ends at the same vertex.

<hr>

## Graph Representation

We represent graphs through:

1. Adjacency Matrix
2. Adjacency List

### Adjacency Matrix

An Adjacency matrix is represented through a 2-dimensional array. If there are n vertices, then we are looking at an n x n Boolean matrix

Each Row and column represents each vertex of the data structure. The elements of both the column and the row must add up to 1 if there is an edge that connects the two, or zero if there isn’t a connection.

* a ***sparse*** graph is when there are very few connections. a ***dense*** graph is when there are many connections

* Within an adjacency matrix, an undirected graph will always be **symmetric**. This is not the case for a directed graph.

### Adjacency List

An adjacency list is the most common way to represent graphs.

An adjacency list is a collection of linked lists or array that lists all of the other vertices that are connected.

Adjacency lists make it easy to view if one vertices connects to another.

<hr>

## Weighted Graphs
A weighted graph is a graph with numbers assigned to its edges. These numbers are called weights.

* When representing a weighted graph in a matrix, you set the element in the 2D array to represent the actual weight between the two paths. If there is not a connection between the two vertices, you can put a 0, although it is known for some people to put the infinity sign instead.

* Within adjacency lists, you must include both the weight and the name of the adjacent vertex.

<hr>


## Traversals

### Breadth First

In a breadth first traversal, you are starting at a specific vertex/node. This node must be specified when calling the `BreadthFirst()` method. The breadth-first traversal of a graph is like that of a tree, with the exception that graphs can have cycles. Traversing a graph that has cycles will result in an infinite loop….this is bad. To prevent such behavior, we need to have some way to keep track of whether a vertex has been “visited” before. Upon each visit, we’ll add the previously-unvisited vertex to a `visited` set, so we know not to visit it again as traversal continues.

***Algorithm:***

1. `Enqueue` the declared start node into the Queue.
2. Create a loop that will run while the node still has nodes present.
3. `Dequeue` the first node from the queue
4. if the Dequeue‘d node has unvisited child nodes, add the unvisited children to `visited` set and insert them into the queue.

***Psuedu code***

```d
ALGORITHM BreadthFirst(vertex)
    DECLARE nodes <-- new List()
    DECLARE breadth <-- new Queue()
    DECLARE visited <-- new Set()

    breadth.Enqueue(vertex)
    visited.Add(vertex)

    while (breadth is not empty)
        DECLARE front <-- breadth.Dequeue()
        nodes.Add(front)

        for each child in front.Children
            if(child is not visited)
                visited.Add(child)
                breadth.Enqueue(child)   

    return nodes;
```

### Depth First

In a depth first traversal, we approach it a bit different than the way we do when working with a depth first traversal of a tree. Similar to how the breadth-first uses a queue, we are going to use a Stack for our depth-first traversal.

***Algorithm:***

1. `Push` the root node into the stack
2. Start a while loop while the stack is not empty
3. `Peek` at the top node in the stack
4. If the top node has unvisited children, mark the top node as visited, and then `Push` any unvisited children back into the stack.
5. If the top node does not have any unvisited children, `Pop` that node off the stack
6. repeat until the stack is empty.


<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/graphs.html)
###### [resource2](https://www.geeksforgeeks.org/iterative-depth-first-traversal/)
###### [resource3](https://www.geeksforgeeks.org/depth-first-search-or-dfs-for-a-graph/)
<!-- ###### [resource4]() -->