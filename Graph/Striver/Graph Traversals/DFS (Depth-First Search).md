```java
import java.util.ArrayList;
import java.util.List;

public class Graph {
    static int vertices = 4; // Number of vertices
    static List<List<Integer>> adj; // Adjacency list

    // Method to perform DFS traversal starting from a given vertex
    static void DFS(int v) {
        boolean[] visited = new boolean[vertices]; // Array to mark visited vertices
        DFSUtil(v, visited); // Call the utility method for DFS
    }

    // Utility method for DFS traversal
    static void DFSUtil(int v, boolean[] visited) {
        visited[v] = true; // Mark the current vertex as visited
        System.out.print(v + " "); // Print the current vertex

        // Recur for all the vertices adjacent to this vertex
        for (int n : adj.get(v)) {
            if (!visited[n]) {
                DFSUtil(n, visited); // Recur if the vertex is not visited
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
        for (int i = 0; i < v; i++) { // Changed from `i <= vertices` to `i < v` to avoid out-of-bounds
            adj.add(new ArrayList<>()); // Initialize each adjacency list
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

        System.out.println("Starting with vertex 2");
        DFS(2); // Perform DFS starting from vertex 2
    }
}

```