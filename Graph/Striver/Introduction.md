## What is Graph Data Structure?

Graph is a non-linear data structure consisting of vertices and edges. The vertices are sometimes also referred to as nodes and the edges are lines or arcs that connect any two nodes in the graph. More formally a [Graph](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/) is composed of a set of vertices( ****V**** ) and a set of edges( ****E**** ). The graph is denoted by ****G(V, E).****


Graph data structures are a powerful tool for representing and analyzing complex relationships between objects or entities. They are particularly useful in fields such as social network analysis, recommendation systems, and computer networks. In the field of sports data science, graph data structures can be used to analyze and understand the dynamics of team performance and player interactions on the field.

## Components of a Graph:

- ****Vertices:**** Vertices are the fundamental units of the graph. Sometimes, vertices are also known as vertex or nodes. Every node/vertex can be labeled or unlabeled.
- ****Edges:**** Edges are drawn or used to connect two nodes of the graph. It can be ordered pair of nodes in a directed graph. Edges can connect any two nodes in any possible way. There are no rules. Sometimes, edges are also known as arcs. Every edge can be labelled/unlabelled.

## Basic Operations on Graphs:

Below are the basic operations on the graph:

- Insertion of Nodes/Edges in the graph – Insert a node into the graph.
- Deletion of Nodes/Edges in the graph – Delete a node from the graph.
- Searching on Graphs – Search an entity in the graph.
- Traversal of Graphs – Traversing all the nodes in the graph.

## Applications of Graph:

#### Following are the real-life applications:

-  Graph data structures can be used to represent the interactions between players on a team, such as passes, shots, and tackles. Analyzing these interactions can provide insights into team dynamics and areas for improvement.
- Commonly used to represent social networks, such as networks of friends on social media.
- Graphs can be used to represent the topology of computer networks, such as the connections between routers and switches.
- Graphs are used to represent the connections between different places in a transportation network, such as roads and airports.
- Graphs are used in Neural Networks where vertices represent neurons and edges represent the synapses between them. Neural networks are used to understand how our brain works and how connections change when we learn. The human brain has about 10^11 neurons and close to 10^15 synapses.
 A graph consists of:

1. **Vertices (or Nodes)**: These are the individual elements or points in the graph. Each vertex can store data and may have a label or identifier.

2. **Edges (or Links)**: These are the connections between the vertices. An edge may be directed (pointing from one vertex to another) or undirected (a bi-directional connection). Edges can also have weights or costs associated with them, representing the strength or capacity of the connection.

### Graphs can be classified based on various properties:

1. **Directed vs. Undirected Graphs**:
   - **Directed Graph (Digraph)**: Edges have a direction, going from one vertex to another.
   - **Undirected Graph**: Edges have no direction, indicating a mutual relationship.

2. **Weighted vs. Unweighted Graphs**:
   - **Weighted Graph**: Edges have weights or costs associated with them.
   - **Unweighted Graph**: Edges do not have weights.

3. **Cyclic vs. Acyclic Graphs**:
   - **Cyclic Graph**: Contains at least one cycle, a path where the start and end vertices are the same.
   - **Acyclic Graph**: Does not contain any cycles.
     - **Directed Acyclic Graph (DAG)**: A directed graph with no cycles.

4. **Connected vs. Disconnected Graphs**:
   - **Connected Graph**: There is a path between any two vertices.
   - **Disconnected Graph**: Some vertices are not connected by paths.

### Representations of Graphs

Graphs can be represented in several ways, the most common being:

1. **Adjacency Matrix**: A 2D array where the cell at row \( i \) and column \( j \) represents the presence (and possibly the weight) of an edge between vertices \( i \) and \( j \). Suitable for dense graphs.

2. **Adjacency List**: An array of lists. Each index represents a vertex, and the list at that index contains the adjacent vertices (and possibly the weights of the edges). Suitable for sparse graphs.

### Applications of Graphs

Graphs are widely used in various applications, including:
- Social networks (modeling relationships between people)
- Computer networks (modeling connections between devices)
- Transportation networks (modeling routes and connections)
- Web page linking (modeling hyperlinks between web pages)
- Dependency graphs (modeling dependencies between tasks or components)

Graphs are powerful tools for representing and solving many real-world problems, providing a structured way to analyze and manage complex relationships.


## Degree
In graph theory, the degree of a vertex is an important concept that refers to the number of edges connected to the vertex. There are two types of degrees, depending on whether the graph is directed or undirected:

### Undirected Graphs

In an undirected graph, the degree of a vertex is simply the number of edges incident to it. This is also called the **degree** or **valence** of the vertex.

- **Degree of a Vertex (v)**: Denoted as \( \text{deg}(v) \), it is the number of edges connected to vertex \( v \).

### Directed Graphs

In a directed graph, each edge has a direction, so there are two types of degrees for each vertex:

1. **In-Degree**: The number of incoming edges to a vertex.
   - **In-Degree of a Vertex (v)**: Denoted as \( \text{deg}^-(v) \), it is the number of edges directed towards vertex \( v \).

2. **Out-Degree**: The number of outgoing edges from a vertex.
   - **Out-Degree of a Vertex (v)**: Denoted as \( \text{deg}^+(v) \), it is the number of edges directed away from vertex \( v \).

### Examples

1. **Undirected Graph Example**:
   - Consider an undirected graph with vertices \( A, B, \) and \( C \).
   - If vertex \( A \) is connected to vertices \( B \) and \( C \), then \( \text{deg}(A) = 2 \).

2. **Directed Graph Example**:
   - Consider a directed graph with vertices \( A, B, \) and \( C \).
   - If there is an edge from \( A \) to \( B \) and an edge from \( A \) to \( C \), then:
     - \( \text{deg}^+(A) = 2 \) (out-degree of \( A \))
     - \( \text{deg}^-(A) = 0 \) (in-degree of \( A \))
     - \( \text{deg}^-(B) = 1 \) (in-degree of \( B \))
     - \( \text{deg}^+(B) = 0 \) (out-degree of \( B \))

### Properties of Degrees

- In an undirected graph, the sum of the degrees of all vertices is twice the number of edges, because each edge contributes to the degree of two vertices.
  \[
  \sum_{v \in V} \text{deg}(v) = 2|E|
  \]
  where \( |E| \) is the number of edges.

- In a directed graph, the sum of all in-degrees is equal to the sum of all out-degrees, which is also equal to the number of edges.
  \[
  \sum_{v \in V} \text{deg}^-(v) = \sum_{v \in V} \text{deg}^+(v) = |E|
  \]

Understanding the degree of vertices is crucial for analyzing the structure and properties of graphs, such as connectivity, centrality, and network robustness.