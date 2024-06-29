
## Adjacency Matrix

```java
public class Main {
    public static void main(String[] args) {

        int n = 3; // number of vertices

        // Initialize the adjacency matrix
        int[][] adj = new int[n + 1][n + 1];

        // Adding edges to the graph
	    
	    // edge 1---2
        adj[1][2] = 1;
        adj[2][1] = 1;

		// edge 2---3
        adj[2][3] = 1;
        adj[3][2] = 1;

		// edge 1---3
        adj[1][3] = 1;
        adj[3][1] = 1;

        // Print the adjacency matrix
        System.out.println("Adjacency Matrix:");
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                System.out.print(adj[i][j] + " ");
            }
            System.out.println();
        }
    }
}

```


> This approach is not recommended because it is costly


## Adjacency matrix

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        int n = 3; // number of vertices

        List<List<Integer>> adj = new ArrayList<>();
        
        // Initialize the adjacency list
        for (int i = 0; i <= n; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Adding edges to the graph
		
		// adj.get(u).add(v);
		// adj.get(v).add(u);
		
		// edge 1---2
        adj.get(1).add(2);
        adj.get(2).add(1);

		// edge 2---3
        adj.get(2).add(3);
        adj.get(3).add(2);
        
        // edge 1---3
        adj.get(1).add(3);
        adj.get(3).add(1);
        
        // Print the adjacency list
        System.out.println("Adjacency List:");
        for (int i = 1; i <= n; i++) {
            System.out.print(i + ": ");
            for (int j = 0; j < adj.get(i).size(); j++) {
                System.out.print(adj.get(i).get(j) + " ");
            }
            System.out.println();
        }
    }
}

```


#### What if we have directed graph?

> Remove the vice-versa nodes and the remaining code will be same.

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        int n = 3; // number of vertices

        List<List<Integer>> adj = new ArrayList<>();
        
        // Initialize the adjacency list
        for (int i = 0; i <= n; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Adding edges to the graph
        adj.get(1).add(2);

        
        adj.get(2).add(3);

        
        adj.get(1).add(3);

        
        // Print the adjacency list
        System.out.println("Adjacency List:");
        for (int i = 1; i <= n; i++) {
            System.out.print(i + ": ");
            for (int j = 0; j < adj.get(i).size(); j++) {
                System.out.print(adj.get(i).get(j) + " ");
            }
            System.out.println();
        }
    }
}
```


### Calculate the Degree of nodes

```java
private void solve(int[][] adj)
    {
        Map<Integer,Integer> nodeFreq  = new HashMap<>();

        // calculate the degree of each node
        for (int[] r: roads)
        {
            int s = r[0]; // s --> source
            int d = r[1]; // d --> destination

            nodeFreq.put(s,nodeFreq.getOrDefault(s,0)+1);
            nodeFreq.put(d,nodeFreq.getOrDefault(d,0)+1);
        }
    }
```