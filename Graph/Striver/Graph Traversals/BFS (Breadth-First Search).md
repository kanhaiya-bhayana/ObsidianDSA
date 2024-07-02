
```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class Graph {
    static int vertices = 4; // Number of vertices
    static List<List<Integer>> adj; // Adjacency list

    // Method to perform BFS traversal starting from a given vertex
    static void BFS(int start) {
        boolean[] visited = new boolean[vertices]; // Array to mark visited vertices
        Queue<Integer> queue = new LinkedList<>(); // Queue for BFS

        visited[start] = true; // Mark the starting vertex as visited
        queue.add(start); // Enqueue the starting vertex

        while (!queue.isEmpty()) {
            int v = queue.poll(); // Dequeue a vertex from the queue
            System.out.print(v + " "); // Print the dequeued vertex

            // Get all adjacent vertices of the dequeued vertex v
            // If an adjacent vertex has not been visited, mark it visited and enqueue it
            for (int n : adj.get(v)) {
                if (!visited[n]) {
                    visited[n] = true; // Mark the vertex as visited
                    queue.add(n); // Enqueue the vertex
                }
            }
        }
    }
    // Method to construct the graph
    static void constructGraph() {
        addEdge(0, 1);
        addEdge(0, 2);
        addEdge(1, 2);
        addEdge(2, 0);
        addEdge(2, 3);
        addEdge(3, 3);
    }

    // Method to initialize the adjacency list
    static void initList(int v) {
        for (int i = 0; i < v; i++) { // Initialize each adjacency list
            adj.add(new ArrayList<>());
        }
    }

    // Method to add an edge to the graph
    static void addEdge(int v, int w) {
        adj.get(v).add(w); // Add w to v's list
    }

    public static void main(String[] args) {
        System.out.println("Hello World");

        adj = new ArrayList<>(); // Initialize the adjacency list
        initList(vertices); // Initialize the list with the number of vertices
        constructGraph(); // Construct the graph with edges

        System.out.println("Starting DFS with vertex 2");
        DFS(2); // Perform DFS starting from vertex 2
        System.out.println();

        System.out.println("Starting BFS with vertex 2");
        BFS(2); // Perform BFS starting from vertex 2
    }
}
```