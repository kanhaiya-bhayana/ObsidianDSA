## 1. BFS of graph
https://www.geeksforgeeks.org/problems/bfs-traversal-of-graph/1

```java
class Solution {
    // Function to return Breadth First Traversal of given graph.
    public ArrayList<Integer> res;
    int v;
    ArrayList<ArrayList<Integer>> adj;
    public ArrayList<Integer> bfsOfGraph(int V, ArrayList<ArrayList<Integer>> adjInput) {
        // Code here
        res = new ArrayList<>();
        adj = adjInput;
        v = V;
        BFS(0);
        return res;
    }
    
    private void BFS(int start)
    {
        // create visited array
        boolean[] visited = new boolean[v];
        
        visited[start] = true;
        
        Queue<Integer> q = new LinkedList<>();
        
        q.add(start);
        
        while (!q.isEmpty())
        {
            int v = q.poll();
            res.add(v);
            for (int e : adj.get(v))
            {
                if (!visited[e])
                {
                    q.add(e);
                    visited[e] = true;
                }
            }
        }
    }
}
```

## 2. DFS of Graph
https://www.geeksforgeeks.org/problems/depth-first-traversal-for-a-graph/1

```java
class Solution {
    // Function to return a list containing the DFS traversal of the graph.
    ArrayList<Integer> res;
    ArrayList<ArrayList<Integer>> adj;
    public ArrayList<Integer> dfsOfGraph(int V, ArrayList<ArrayList<Integer>> adjList) {
        // Code here
        res = new ArrayList<>();
        adj = adjList;
        boolean[] visited = new boolean[V];
        
        DFS(0, visited);
        return res;
    }
    
    private void DFS(int start, boolean[] visited)
    {
        visited[start] = true;
        res.add(start);
        
        for (int n : adj.get(start))
        {
            if (!visited[n])
            {
                DFS(n, visited);
            }
        }
    }
}
```

## 3. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

```java
class Solution {
    List<List<Integer>> adj;
    public int findCircleNum(int[][] isConnected) {
        constructAdjList(isConnected);
        return solve(adj.size());
    }

    private int solve(int V)
    {
        boolean[] visited = new boolean[V];
        int cnt = 0;
        for (int i = 0; i < V; i++)
        {
            if (!visited[i])
            {
                cnt++;
                dfs(i, visited);
            }
        }
        return cnt;
    }

    private void dfs(int v, boolean[] visited)
    {
        visited[v] = true;

        for (int n : adj.get(v))
        {
            if (!visited[n])
            {
                visited[n] = true;
                dfs(n, visited);
            }
        }
    }

    private void constructAdjList(int[][] arr)
    {
        int V = arr.length;
        adj = new ArrayList<>();

        for (int i = 0; i < V; i++)
        {
            adj.add(new ArrayList<>());
        }

        for (int i = 0; i < V; i++) {
            for (int j = 0; j < V; j++) {
                if (arr[i][j] == 1 && i != j)
                {
                    adj.get(i).add(j);
                    adj.get(j).add(i);
                }
            }
        }
    }
}
```


## 4. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

```java
class Solution {
    int rows;
    int cols;
    public int numIslands(char[][] grid) {
        
        rows = grid.length;
        cols = grid[0].length;
        return numIslandsUtil(grid);
    }

    private int numIslandsUtil(char [][]grid){

        if (grid == null || grid.length == 0){
            return 0;
        }
        int numIslands = 0;
        
        for (int i = 0; i < rows; i++){
            for (int j = 0; j < cols; j++){
                if (grid[i][j] == '1'){
                    numIslands++;
                    dfs(grid, i, j);
                }
            }
        }
        return numIslands;
    }


    private void dfs(char [][]grid, int i, int j){

        if (i < 0 || i >= rows 
         || j < 0 || j >= cols
         || grid[i][j] != '1')
         {
            return;
         }

         grid[i][j] = '0'; // mark as visited ...
         dfs(grid, i + 1, j); // down ...
         dfs(grid, i - 1, j); // up ...
         dfs(grid, i, j + 1); // right ...
         dfs(grid, i, j - 1); // left ...
    }
}
```

## 5. [Flood Fill](https://leetcode.com/problems/flood-fill/)

```java
class Solution {
    int rows;
    int cols;
    int clr;
    int originalColor;

    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        // Initialize rows, cols, new color, and original color
        rows = image.length;
        cols = image[0].length;
        clr = color;
        originalColor = image[sr][sc];

        // Only perform flood fill if the new color is different from the original color
        if (originalColor != clr) {
            dfs(image, sr, sc);
        }

        // Return the modified image
        return image;
    }

    private void dfs(int[][] grid, int i, int j) {
        // Check for out of bounds or if the current cell is not of the original color
        if (i < 0 || i >= rows || j < 0 || j >= cols || grid[i][j] != originalColor) {
            return;
        }

        // Fill the current cell with the new color
        grid[i][j] = clr;

        // Recursively call dfs on the neighboring cells (up, down, left, right)
        dfs(grid, i + 1, j); // Down
        dfs(grid, i - 1, j); // Up
        dfs(grid, i, j + 1); // Right
        dfs(grid, i, j - 1); // Left
    }
}
```

## 6. Rotten Oranges
https://www.geeksforgeeks.org/problems/rotten-oranges2536/1

```java
class Solution {
    // Nested Pair class to hold row, column, and time information
    class Pair {
        int row;
        int col;
        int tm;

        // Constructor to initialize row, column, and time
        Pair(int _row, int _col, int _tm) {
            this.row = _row;
            this.col = _col;
            this.tm = _tm;
        }
    }

    // Function to find minimum time required to rot all oranges
    public int orangesRotting(int[][] grid) {
        return solve(grid); // Delegate the logic to solve() method
    }

    private int solve(int[][] grid) {
        int n = grid.length; // Number of rows
        int m = grid[0].length; // Number of columns

        Queue<Pair> q = new LinkedList<>(); // Queue to store rotten oranges and their time
        boolean[][] vis = new boolean[n][m]; // Visited array to track processed cells
        int cntFresh = 0; // Count of fresh oranges

        // Traverse the grid to initialize the queue with rotten oranges and count fresh oranges
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 2) {
                    q.add(new Pair(i, j, 0)); // Add rotten orange to queue with time 0
                    vis[i][j] = true; // Mark it as visited
                }
                if (grid[i][j] == 1) {
                    cntFresh++; // Increment count of fresh oranges
                }
            }
        }

        int tm = 0; // Time to rot all oranges
        int[] drow = {-1, 0, +1, 0}; // Row direction vectors (up, right, down, left)
        int[] dcol = {0, +1, 0, -1}; // Column direction vectors (up, right, down, left)
        int cnt = 0; // Count of rotted fresh oranges

        // Process the queue until empty
        while (!q.isEmpty()) {
            int row = q.peek().row; // Current row
            int col = q.peek().col; // Current column
            int t = q.peek().tm; // Current time
            tm = Math.max(t, tm); // Update maximum time

            q.poll(); // Remove the processed orange from queue

            // Traverse in all 4 directions
            for (int i = 0; i < 4; i++) {
                int nrow = row + drow[i]; // Calculate new row
                int ncol = col + dcol[i]; // Calculate new column

                // Check if the new cell is within bounds and not visited, and if it's a fresh orange
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !vis[nrow][ncol] && grid[nrow][ncol] == 1) {
                    q.offer(new Pair(nrow, ncol, t + 1)); // Add the new rotten orange to the queue with incremented time
                    vis[nrow][ncol] = true; // Mark it as visited
                    cnt++; // Increment count of rotted fresh oranges
                }
            }
        }

        // If not all fresh oranges have rotted, return -1
        if (cnt != cntFresh) {
            return -1;
        }
        return tm; // Return the total time taken to rot all oranges
    }
}
```

## 7. Undirected Graph Cycle
https://www.geeksforgeeks.org/problems/detect-cycle-in-an-undirected-graph/1
```java
class Solution {
    // Function to detect cycle in an undirected graph.
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited = new boolean[V]; // Array to keep track of visited nodes
        // Iterate through all vertices to ensure all components are checked
        for (int i = 0; i < V; i++) {
            // If the node is not visited, check for a cycle in its component
            if (!visited[i]) {
                if (solve(i, visited, adj)) {
                    return true; // If any component has a cycle, return true
                }
            }
        }
        return false; // If no component has a cycle, return false
    }

    // Helper function to perform BFS and detect a cycle in the graph component
    private boolean solve(int s, boolean[] visited, ArrayList<ArrayList<Integer>> adj) {
        Queue<Pair> q = new LinkedList<>(); // Queue for BFS

        // Start BFS from the given node
        q.offer(new Pair(s, -1)); // Add the starting node with no parent (-1)
        visited[s] = true; // Mark the starting node as visited

        // Process the queue until it is empty
        while (!q.isEmpty()) {
            int node = q.peek().node; // Current node
            int par = q.peek().par; // Parent of the current node
            
            q.poll(); // Remove the current node from the queue
            // Traverse all adjacent nodes
            for (int it : adj.get(node)) {
                // If the adjacent node is not visited, add it to the queue
                if (!visited[it]) {
                    q.offer(new Pair(it, node)); // Add the adjacent node with the current node as its parent
                    visited[it] = true; // Mark the adjacent node as visited
                } 
                // If the adjacent node is visited and is not the parent, a cycle is detected
                else if (par != it) {
                    return true;
                }
            }
        }
        return false; // No cycle detected in this component
    }

    // Pair class to hold the node and its parent
    class Pair {
        int node;
        int par;

        Pair(int n, int p) {
            node = n;
            par = p;
        }
    }
}

```