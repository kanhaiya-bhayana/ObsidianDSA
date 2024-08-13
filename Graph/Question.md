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

###### Explanation
- **Grid Initialization**: The grid is initialized with the input matrix in the `numIslands` method. The number of rows and columns are also stored for later use.
- **Counting Islands**: The `solve` method iterates through each cell in the grid. If the cell is a part of an island (i.e., contains '1') and hasn't been visited, it increments the island count and performs a BFS to mark all the connected parts of the island as visited.

- **Breadth-First Search (BFS)**: The BFS method is used to explore all connected cells that make up an island. It uses a queue to visit each cell level by level, ensuring that all parts of the island are accounted for.

- **Helper Class (Pair)**: The `Pair` class is a simple helper to store row and column indices together, making it easier to manage the grid cells during BFS.

This algorithm effectively counts the number of distinct islands in the given grid by exploring each island fully and marking it as visited to avoid counting it multiple times.

```java
class Solution {
    // Grid to store the map of islands and water
    char[][] grid;
    // Number of rows and columns in the grid
    int rows; 
    int cols;

    // Method to calculate the number of islands in the grid
    public int numIslands(char[][] mat) {
        // Initialize the grid with the input matrix
        grid = mat;
        // Get the number of rows and columns in the grid
        rows = grid.length;
        cols = grid[0].length;

        // Call the solve method to count the islands
        return solve(rows, cols);
    }

    // Method to count the number of islands in the grid
    private int solve(int n, int m) {
        // 2D array to keep track of visited cells
        boolean[][] visited = new boolean[n][m];
        
        // Counter for the number of islands
        int cnt = 0;

        // Iterate over each cell in the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                // If the cell is part of an island ('1') and hasn't been visited
                if (!visited[i][j] && grid[i][j] == '1') {
                    // Increment the island counter
                    cnt++;
                    // Perform BFS to visit all connected parts of the island
                    BFS(i, j, visited);
                }
            }
        }
        // Return the total number of islands
        return cnt;
    }

    // BFS method to explore all parts of an island starting from (row, col)
    private void BFS(int row, int col, boolean[][] visited) {
        // Mark the starting cell as visited
        visited[row][col] = true;
        // Queue to manage the BFS traversal, initialized with the starting cell
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(row, col));

        // Directions for moving in the grid (up, down, left, right)
        int[] rowDelta = {-1, 1, 0, 0};
        int[] colDelta = {0, 0, -1, 1};

        // Process the queue until all connected cells are visited
        while (!q.isEmpty()) {
            // Get the current cell from the queue
            Pair it = q.poll();
            int r = it.row;
            int c = it.col;

            // Explore all 4 possible directions from the current cell
            for (int i = 0; i < 4; i++) {
                int nbr = r + rowDelta[i];
                int nbc = c + colDelta[i];

                // Check if the neighboring cell is within bounds, part of an island, and not visited
                if (nbr >= 0 && nbr < rows && nbc >= 0 && nbc < cols && grid[nbr][nbc] == '1' && !visited[nbr][nbc]) {
                    // Mark the neighboring cell as visited
                    visited[nbr][nbc] = true;
                    // Add the neighboring cell to the queue to continue BFS
                    q.offer(new Pair(nbr, nbc));
                }
            }
        }
    }

    // Helper class to represent a cell's row and column in the grid
    class Pair {
        int row;
        int col;

        // Constructor to initialize the row and column
        Pair(int r, int c) {
            row = r;
            col = c;
        }
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

###### Using DFS
```java
class Solution {
    ArrayList<ArrayList<Integer>> adj;

    // Function to detect cycle in an undirected graph.
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adjList) {
        // Initialize the adjacency list for the graph.
        adj = adjList;

        // Array to keep track of visited nodes.
        boolean[] visited = new boolean[V];

        // Iterate over all vertices.
        for (int i = 0; i < V; i++) {
            // If the vertex is not visited, perform DFS.
            if (!visited[i]) {
                // Check if a cycle is found starting from this vertex.
                if (dfs(i, -1, visited)) {
                    return true; // Cycle found.
                }
            }
        }
        return false; // No cycle found in the graph.
    }
    
    // Helper function for DFS traversal.
    boolean dfs(int node, int parent, boolean[] visited) {
        // Mark the current node as visited.
        visited[node] = true;

        // Iterate over all the adjacent nodes.
        for (int it : adj.get(node)) {
            // If the adjacent node is not visited, perform DFS on it.
            if (!visited[it]) {
                // Recursive DFS call.
                if (dfs(it, node, visited)) {
                    return true; // Cycle found.
                }
            }
            // If the adjacent node is visited and is not the parent of the current node, a cycle is found.
            else if (it != parent) {
                return true; // Cycle found.
            }
        }
        return false; // No cycle found from the current node.
    }
}
```

## 8. Distance of nearest cell having 1
https://www.geeksforgeeks.org/problems/distance-of-nearest-cell-having-1-1587115620/1
```java
class Solution
{
    // Inner class to represent a node with row, column, and distance
    class Node
    {
        int first;  // Row index
        int second; // Column index
        int third;  // Distance from the nearest 1

        Node(int f, int s, int t)
        {
            first = f;
            second = s;
            third = t;
        }
    }

    // Function to find the distance of the nearest 1 in the grid for each cell.
    public int[][] nearest(int[][] grid)
    {
        // Start BFS to calculate distances
        return BFS(grid);
    }
    
    // BFS function to calculate the shortest distance to the nearest 1 for each cell
    private int[][] BFS(int[][] grid)
    {
        int n = grid.length; // Number of rows in the grid
        int m = grid[0].length; // Number of columns in the grid
        
        int[][] dist = new int[n][m]; // Resultant distance matrix
        boolean[][] visited = new boolean[n][m]; // Visited array to keep track of visited cells
        Queue<Node> q = new LinkedList<>(); // Queue for BFS
        
        // Initialize the queue with all cells containing 1 and mark them as visited
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (grid[i][j] == 1)
                {
                    q.offer(new Node(i, j, 0)); // Enqueue cells with 1 and distance 0
                    visited[i][j] = true; // Mark these cells as visited
                }
            }
        }
        
        // Directions for moving up, right, down, and left
        int[] delRow = {-1, 0, +1, 0};
        int[] delCol = {0, +1, 0, -1};
        
        // BFS traversal
        while(!q.isEmpty())
        {
            // Get the current node's row, column, and distance
            int row = q.peek().first;
            int col = q.peek().second;
            int steps = q.peek().third;
            q.poll(); // Remove the current node from the queue
            dist[row][col] = steps; // Set the distance for the current cell
            
            // Explore all four possible directions
            for (int i = 0; i < 4; i++)
            {
                int nrow = row + delRow[i]; // New row index
                int ncol = col + delCol[i]; // New column index
                
                // Check if the new cell is within bounds and not visited
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !visited[nrow][ncol])
                {
                    visited[nrow][ncol] = true; // Mark the new cell as visited
                    q.offer(new Node(nrow, ncol, steps + 1)); // Add the new cell to the queue with incremented distance
                }
            }
        }
        return dist; // Return the resultant distance matrix
    }
}

```

## 9. [01 Matrix](https://leetcode.com/problems/01-matrix/)

```java
class Solution {
    class Node{
        int first;
        int second;
        int third;
        
        Node(int f, int s, int t)
        {
            first = f;
            second = s;
            third = t;
        }
    }
    public int[][] updateMatrix(int[][] mat) {
        return BFS(mat);
    }

    private int[][] BFS(int[][] grid)
    {
        int n = grid.length;
        int m = grid[0].length;
        
        int[][] dist = new int[n][m];
        boolean[][] visited = new boolean[n][m];
        
        Queue<Node> q = new LinkedList<>();
        
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (grid[i][j] == 0)
                {
                    visited[i][j] = true;
                    q.offer(new Node(i,j,0));
                }
            }
        }
        
        int[] delRow = {-1, 0, +1, 0};
        int[] delCol = {0, +1, 0, -1};
        
        while (!q.isEmpty())
        {
            int row = q.peek().first;
            int col = q.peek().second;
            int steps = q.peek().third;
            
            q.poll();
            dist[row][col] = steps;
            
            for(int i = 0; i < 4; i++)
            {
                int nrow = row + delRow[i];
                int ncol = col + delCol[i];
                
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !visited[nrow][ncol])
                {
                    visited[nrow][ncol] = true;
                    q.offer(new Node(nrow,ncol, steps+1));
                }
            }
        }
        return dist;
    }
}
```

## 10. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

```java
class Solution {

    public void solve(char[][] board) {
        solve2(board);
    }

    private void solve2(char[][] mat)
    {
        int n = mat.length;
        int m = mat[0].length;
        boolean[][] visited = new boolean[n][m];

        // traverse first row and last row
        for (int j = 0; j < m; j++)
        {
            // first row
            if (!visited[0][j] && mat[0][j] == 'O')
            {
                dfs(0,j,visited,mat,n,m);
            }

            // last row 
            if (!visited[n-1][j] && mat[n-1][j] == 'O')
            {
                dfs(n-1,j,visited,mat,n,m);
            }
        }

        // traverse first col and last col
        for (int i = 0; i < n; i++)
        {
            // first row
            if (!visited[i][0] && mat[i][0] == 'O')
            {
                dfs(i,0,visited,mat,n,m);
            }

            // last row 
            if (!visited[i][m-1] && mat[i][m-1] == 'O')
            {
                dfs(i,m-1,visited,mat,n,m);
            }
        }
        // traverse entire stuff
        for (int i = 0; i<n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (!visited[i][j] && mat[i][j] == 'O')
                {
                    mat[i][j] = 'X';
                }
            }
        }
    }


    private void dfs(int row, int col, boolean[][] vis, char[][] mat, int n, int m)
    {
        // check for top, right, bottom, left
        vis[row][col] = true;
        int[] delRow = {-1, 0, +1, 0};
        int[] delCol = {0, +1, 0, -1};

        for (int i = 0; i< 4; i++)
        {
            int nrow = row + delRow[i];
            int ncol = col + delCol[i];

            if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !vis[nrow][ncol] && mat[nrow][ncol] == 'O')
            {
                dfs(nrow, ncol, vis, mat, n, m);
            }
        }
    }
}
```

## 11. [Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/)

```java
class Solution {
    // Helper class to represent a cell in the grid with its row and column indices
    class Pair {
        int first;
        int second;
        Pair(int f, int s) {
            first = f;
            second = s;
        }
    }

    // Main function to find the number of enclaves
    public int numEnclaves(int[][] grid) {
        return solve(grid);
    }

    // Helper function to perform the core logic
    private int solve(int[][] grid) {
        int n = grid.length; // Number of rows in the grid
        int m = grid[0].length; // Number of columns in the grid
        Queue<Pair> q = new LinkedList<>(); // Queue for BFS
        boolean[][] vis = new boolean[n][m]; // Visited array to keep track of visited cells

        // Mark boundary land cells and add them to the queue
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (i == 0 || j == 0 || i == n-1 || j == m-1) {
                    if (grid[i][j] == 1) {
                        q.offer(new Pair(i, j));
                        vis[i][j] = true; // Mark the cell as visited
                    }
                }
            }
        }

        // Direction arrays to move up, right, down, and left
        int[] delRow = {-1, 0, +1, 0};
        int[] delCol = {0, +1, 0, -1};

        // Perform BFS to mark all reachable land cells from the boundary
        while (!q.isEmpty()) {
            int row = q.peek().first;
            int col = q.peek().second;
            q.poll();

            // Explore all 4 possible directions
            for (int i = 0; i < 4; i++) {
                int nrow = row + delRow[i];
                int ncol = col + delCol[i];

                // If the new cell is within bounds, is land, and not visited
                if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !vis[nrow][ncol] && grid[nrow][ncol] == 1) {
                    q.offer(new Pair(nrow, ncol)); // Add the new cell to the queue
                    vis[nrow][ncol] = true; // Mark the new cell as visited
                }
            }
        }

        int cnt = 0; // Counter for enclaved land cells
        // Count the number of land cells that are not visited (i.e., enclaved)
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!vis[i][j] && grid[i][j] == 1) {
                    cnt++;
                }
            }
        }
        return cnt; // Return the count of enclaved land cells
    }
}
```

## 12. Number of Distinct Islands
https://www.geeksforgeeks.org/problems/number-of-distinct-islands/1
```java
// User function Template for Java

class Solution {

    int n; // Number of rows in the grid
    int m; // Number of columns in the grid
    
    // Helper class to store coordinates
    class Pair {
        int first; // Row coordinate
        int second; // Column coordinate
        
        Pair(int f, int s) {
            first = f;
            second = s;
        }
    }
    
    // Function to count distinct islands
    int countDistinctIslands(int[][] grid) {
        return solve(grid);
    }
    
    // Main function to solve the problem
    private int solve(int[][] grid) {
        n = grid.length; // Get number of rows
        m = grid[0].length; // Get number of columns
        
        boolean[][] vis = new boolean[n][m]; // Visited array to track visited cells
        Set<String> set = new HashSet<>(); // Set to store unique island shapes
        
        // Iterate through each cell in the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                // If cell is not visited and is part of an island
                if (!vis[i][j] && grid[i][j] == 1) {
                    List<Pair> lst = new ArrayList<>(); // List to store the shape of the current island
                    dfs(i, j, vis, grid, lst, i, j); // Perform DFS to explore the island
                    
                    // Convert island shape to a string
                    String s = "";
                    for (Pair p : lst) {
                        s += p.first + "," + p.second + "+";
                    }
                    set.add(s); // Add island shape to the set
                }
            }
        }
        return set.size(); // Return the number of unique island shapes
    }
    
    // DFS function to explore the island
    private void dfs(int row, int col, boolean[][] vis, int[][] grid, List<Pair> lst, int baseRow, int baseCol) {
        vis[row][col] = true; // Mark the current cell as visited
        lst.add(new Pair(row - baseRow, col - baseCol)); // Add the relative position to the list
        
        // Arrays to explore the 4 possible directions (up, right, down, left)
        int[] delRow = {-1, 0, +1, 0};
        int[] delCol = {0, +1, 0, -1};
        
        // Explore all 4 directions
        for (int i = 0; i < 4; i++) {
            int nrow = row + delRow[i];
            int ncol = col + delCol[i];
            
            // Check if the new position is within bounds and is part of the island
            if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !vis[nrow][ncol] && grid[nrow][ncol] == 1) {
                dfs(nrow, ncol, vis, grid, lst, baseRow, baseCol); // Recursively explore the new position
            }
        }
    }
}
```


---
## Bipartite Graph
---

two adjacent nodes cannot have the same color. 

> Linear Graphs with no cycle are always Bipartite
> Any Graph with even length cycle are always Bipartite
> Any Graph with odd length cycle can never be a Bipartite

## 13. [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

###### Using BFS
 ```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int V = graph.length;
        int[] color = new int[V];
        Arrays.fill(color, -1);

        for (int i = 0; i < V; i++) {
            if (color[i] == -1) {
                if (!BFS(i, graph, color)) {
                    return false;
                }
            }
        }
        return true;
    }

    private boolean BFS(int start, int[][] graph, int[] color) {
        Queue<Integer> q = new LinkedList<>();
        q.offer(start);
        color[start] = 0;

        while (!q.isEmpty()) {
            int node = q.poll();

            for (int neighbor : graph[node]) {
                if (color[neighbor] == -1) {
                    color[neighbor] = 1 - color[node];
                    q.offer(neighbor);
                } else if (color[neighbor] == color[node]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
###### GFG - Bipartite Graph
https://www.geeksforgeeks.org/problems/bipartite-graph/1
```java


class Solution
{
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        // Code here
        int[] color = new int[V];
        for (int i = 0; i < V; i++)
        {
            color[i] = -1;
        }
        
        for (int i = 0; i< V; i++)
        {
            if (color[i] == -1)
            {
                if (BFS(i,V,adj,color) == false)
                {
                    return false;
                }   
            }
        }
        return true;
    }
    
    
    private boolean BFS(int start, int V, ArrayList<ArrayList<Integer>>adj, int[] color)
    {
        Queue<Integer> q = new LinkedList<>();
        q.offer(start);
        color[start] = 0;
        
        while (!q.isEmpty())
        {
            int node = q.peek();
            q.poll();
            
            for (int it : adj.get(node))
            {
                if (color[it] == -1)
                {
                    color[it] = (1 - color[node]);
                    q.offer(it);
                }
                else if (color[it] == color[node])
                {
                    return false;
                }
            }
        }
        return true;
    }
}
```
### USING DFS
https://leetcode.com/problems/is-graph-bipartite/description/
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int V = graph.length;
        int[] color = new int[V];
        Arrays.fill(color, -1);

        for (int i = 0; i < V; i++) {
            if (color[i] == -1) {
                if (!DFS(i,0, graph, color)) {
                    return false;
                }
            }
        }
        return true;
    }
    private boolean DFS(int node, int col, int[][] graph, int[] color)
    {
        color[node] = col;
        
        for (int n : graph[node])
        {
            if (color[n] == -1)
            {
                // color[n] = 1 - color[start];
                if (DFS(n,1-col,graph,color) == false){
                    return false;
                }
            }
            else if (color[n] == col){
                return false;
            }
        }
        return true;
    }
}
```

https://www.geeksforgeeks.org/problems/bipartite-graph/1
```java
class Solution
{
    public boolean isBipartite(int V, ArrayList<ArrayList<Integer>>adj)
    {
        // Code here
        int[] color = new int[V];
        for (int i = 0; i < V; i++)
        {
            color[i] = -1;
        }
        for (int i = 0; i< V; i++)
        {
            if (color[i] == -1)
            {
                if (DFS(i,0,adj,color) == false)
                {
                    return false;
                }   
            }
        }
        return true;
    }
    private boolean DFS(int node, int col, ArrayList<ArrayList<Integer>>adj, int[] color)
    {
        color[node] = col;
        
        for (int n : adj.get(node))
        {
            if (color[n] == -1)
            {
                // color[n] = 1 - color[start];
                if (DFS(n,1-col,adj,color) == false){
                    return false;
                }
            }
            else if (color[n] == col){
                return false;
            }
        }
        return true;
    }
}
```

## 14.  Directed Graph Cycle
https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1
```java
/*Complete the function below*/

class Solution {
    // Function to detect a cycle in a directed graph.
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        // Array to keep track of visited nodes
        boolean[] vis = new boolean[V];
        // Array to keep track of nodes in the current DFS path
        boolean[] visPath = new boolean[V];
        
        // Iterate through all nodes
        for (int i = 0; i < V; i++) {
            // If the node is not visited, perform DFS
            if (!vis[i]) {
                // If a cycle is detected during DFS, return true
                if (dfs(i, adj, vis, visPath)) {
                    return true;
                }
            }
        }
        // If no cycle is detected, return false
        return false;
    }
    
    // Helper function to perform DFS and detect cycles
    private boolean dfs(int start, ArrayList<ArrayList<Integer>> adj, boolean[] vis, boolean[] visPath) {
        // Mark the current node as visited
        vis[start] = true;
        // Mark the current node as part of the current DFS path
        visPath[start] = true;
        
        // Iterate through all adjacent nodes
        for (int it : adj.get(start)) {
            // If the adjacent node is not visited, perform DFS on it
            if (!vis[it]) {
                // If a cycle is detected, return true
                if (dfs(it, adj, vis, visPath)) {
                    return true;
                }
            }
            // If the adjacent node is visited and is part of the current DFS path, a cycle is detected
            else if (visPath[it]) {
                return true;
            }
        }
        // Unmark the current node as part of the current DFS path before returning
        visPath[start] = false;
        return false;
    }
}
```

## 15. Eventual Safe States
https://www.geeksforgeeks.org/problems/eventual-safe-states/1


###### Notes
- **Safe Node:**
- **Terminal Node:** The nodes who does not have any outgoing edge or the node having the outdegree is zero(0).
- Anyone who is a part of a cycle is not a safe node.
- Anyone who leads to a cycle is not a safe node.
```java
class Solution {

    // Function to find all eventual safe nodes in a directed graph.
    List<Integer> eventualSafeNodes(int V, List<List<Integer>> adj) {

        // Array to keep track of visited nodes
        boolean[] vis = new boolean[V];
        // Array to keep track of nodes in the current DFS path
        boolean[] visPath = new boolean[V];
        // Array to mark nodes that are safe
        boolean[] check = new boolean[V];
        // List to store the result of safe nodes
        List<Integer> res = new ArrayList<>();
        
        // Iterate through all nodes
        for (int i = 0; i < V; i++) {
            // If the node is not visited, perform DFS
            if (!vis[i]) {
                // Perform DFS to check for cycles
                dfs(i, adj, vis, visPath, check);
            }
        }
        
        // Collect all safe nodes
        for (int i = 0; i < V; i++) {
            if (check[i]) {
                res.add(i);
            }
        }
        
        // Return the list of safe nodes
        return res;
    }
    
    // Helper function to perform DFS and detect cycles
    private boolean dfs(int start, List<List<Integer>> adj, boolean[] vis, boolean[] visPath, boolean[] check) {
        // Mark the current node as visited
        vis[start] = true;
        // Mark the current node as part of the current DFS path
        visPath[start] = true;
        // Initially mark the current node as unsafe
        check[start] = false;
        
        // Iterate through all adjacent nodes
        for (int it : adj.get(start)) {
            // If the adjacent node is not visited, perform DFS on it
            if (!vis[it]) {
                // If a cycle is detected, mark the node as unsafe and return true
                if (dfs(it, adj, vis, visPath, check)) {
                    check[start] = false;
                    return true;
                }
            }
            // If the adjacent node is visited and is part of the current DFS path, a cycle is detected
            else if (visPath[it]) {
                check[start] = false;
                return true;
            }
        }
        
        // Mark the current node as safe if no cycle is detected
        check[start] = true;
        // Unmark the current node as part of the current DFS path before returning
        visPath[start] = false;
        return false;
    }
}
```

## 16 Topological Sort Algorithm (DFS)
<<<<<<< HEAD

=======
https://www.geeksforgeeks.org/problems/topological-sort/1
>>>>>>> 9c3975b2dd01ac94b914b640ff07d8b317834891
- Only exist on DAG (Directed Acyclic Graph)
- **Definition:** Linear ordering of vertices such that if there is an edge between u and v, u appears before v in that orderring.

```java
/*Complete the function below*/
class Solution
{
    // Function to return list containing vertices in Topological order.
    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) 
    {
        // Calling the helper function to get the topological sort order.
        return solve(V, adj);
    }
    
    // Helper function to perform topological sort.
    private static int[] solve(int V, ArrayList<ArrayList<Integer>> adj)
    {
        // Boolean array to keep track of visited nodes.
        boolean[] vis = new boolean[V];
        // Stack to store the nodes in topological order.
        Stack<Integer> st = new Stack<>();
        
        // Iterate over all nodes.
        for (int i = 0; i < V; i++)
        {
            // If the node is not visited, perform DFS from that node.
            if (!vis[i])
            {
                dfs(i, adj, st, vis);
            }
        }

        // Array to store the result of topological sort.
        int[] res = new int[V];
        int i = 0;

        // Pop elements from the stack to get the topological order.
        while (!st.isEmpty())
        {
            res[i] = st.peek();
            i++;
            st.pop();
        }

        return res;
    }
    
    // Helper function to perform Depth-First Search.
    private static void dfs(int node, ArrayList<ArrayList<Integer>> adj, Stack<Integer> st, boolean[] vis)
    {
        // Mark the current node as visited.
        vis[node] = true;

        // Recur for all the vertices adjacent to this vertex.
        for (int nbs : adj.get(node))
        {
            // If an adjacent node has not been visited, then recur for that adjacent node.
            if (!vis[nbs])
            {
                dfs(nbs, adj, st, vis);
            }
        }

        // Push the current node to stack which stores the result.
        st.push(node);
    }
}
```

## 17 Topological Sort | Khan's Algorithm  (BFS)
https://www.geeksforgeeks.org/problems/topological-sort/1
###### Steps for Topological Sort Algorithm

1. **Initialization**:
    
    - Create a queue to store vertices with indegree 0.
    - Create an array `topo` to store the topological order of vertices.
    - Create an array `indegree` to store the indegree of each vertex.
2. **Calculate Indegrees**:
    
    - Traverse the adjacency list of the graph to calculate the indegree of each vertex.
    - For each vertex `u`, increase the indegree of its adjacent vertices `v`.
3. **Enqueue Vertices with Indegree 0**:
    
    - Traverse the `indegree` array.
    - Add all vertices with indegree 0 to the queue.
4. **Process the Queue**:
    
    - Initialize an index variable to 0.
    - While the queue is not empty:
        - Dequeue a vertex `node` from the front of the queue.
        - Add `node` to the `topo` array at the current index.
        - Increment the index.
        - For each adjacent vertex `neighbor` of `node`:
            - Decrease the indegree of `neighbor` by 1.
            - If the indegree of `neighbor` becomes 0, enqueue `neighbor`.
5. **Return the Result**:
    
    - Return the `topo` array containing the vertices in topological order.

```java


/*Complete the function below*/


class Solution {
    // Function to return a list containing vertices in Topological order
    static int[] topoSort(int V, ArrayList<ArrayList<Integer>> adj) {
        Queue<Integer> queue = new LinkedList<>();
        int[] topo = new int[V];
        
        // Calculate the indegrees of all vertices
        int[] indegree = getIndegrees(adj, V);
        
        // Add all vertices with indegree 0 to the queue
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        int index = 0; // Index to maintain the position in topo array
        while (!queue.isEmpty()) {
            int node = queue.poll();
            topo[index++] = node; // Add the node to the topological order
            
            // Decrease the indegree of adjacent vertices
            for (int neighbor : adj.get(node)) {
                indegree[neighbor]--;
                // If indegree becomes 0, add it to the queue
                if (indegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
        
        return topo; // Return the topological order
    }
    
    // Function to calculate the indegrees of all vertices
    private static int[] getIndegrees(ArrayList<ArrayList<Integer>> adj, int V) {
        int[] indegree = new int[V];
        
        // Traverse the adjacency list to calculate the indegree of each vertex
        for (int i = 0; i < V; i++) {
            for (int neighbor : adj.get(i)) {
                indegree[neighbor]++;
            }
        }
        
        return indegree; // Return the indegree array
    }
}
```

## 18 Detect a cycle in DAG | Topo Sort | Kahn's Algo
https://www.geeksforgeeks.org/problems/detect-cycle-in-a-directed-graph/1
###### Explanation:
1. **Initialization**:
   - A queue `q` is initialized to store vertices that have no incoming edges.
   - The `getIndegrees` function is called to calculate the in-degrees of all vertices.

2. **Adding Vertices with Zero In-degree to Queue**:
   - Vertices with zero in-degrees are added to the queue because they do not depend on any other vertices.

3. **Processing Vertices**:
   - A counter `cnt` is used to keep track of the number of vertices processed.
   - While the queue is not empty, vertices are dequeued, and their adjacent vertices' in-degrees are decremented.
   - If an adjacent vertex's in-degree becomes zero, it is added to the queue.

4. **Cycle Detection**:
   - If `cnt` is equal to `V` (the number of vertices), it means all vertices were processed, indicating there is no cycle.
   - If `cnt` is less than `V`, it means some vertices couldn't be processed due to cyclic dependencies, indicating the presence of a cycle.

5. **Helper Function `getIndegrees`**:
   - This function calculates the in-degree for each vertex by counting the number of incoming edges.

This algorithm is based on Kahn's Algorithm for Topological Sorting, which can also be used to detect cycles in a directed graph. If the topological sort includes all vertices, the graph is acyclic; otherwise, it contains a cycle.
###### Code
```java
class Solution {
    // Function to detect a cycle in a directed graph.
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        // Initialize a queue to store vertices with no incoming edges.
        Queue<Integer> q = new LinkedList<>();
        
        // Get the in-degree of each vertex.
        int[] indegree = getIndegrees(V, adj);
        
        // Add vertices with no incoming edges to the queue.
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }
        
        // Counter for the number of vertices processed.
        int cnt = 0;
        
        // Process vertices in the queue.
        while (!q.isEmpty()) {
            int node = q.poll();
            cnt++;
            
            // Decrease the in-degree of adjacent vertices.
            for (int ele : adj.get(node)) {
                indegree[ele]--;
                
                // If in-degree becomes zero, add the vertex to the queue.
                if (indegree[ele] == 0) {
                    q.offer(ele);
                }
            }
        }
        
        // If the number of processed vertices is equal to the number of vertices,
        // then the graph does not contain a cycle.
        if (cnt == V) return false;
        
        // If the graph contains a cycle, the number of processed vertices will be less than V.
        return true;
    }
    
    // Helper function to calculate the in-degree of each vertex.
    private int[] getIndegrees(int V, ArrayList<ArrayList<Integer>> adj) {
        // Initialize the in-degree array.
        int[] indegree = new int[V];
        
        // Calculate in-degrees by counting incoming edges for each vertex.
        for (int i = 0; i < V; i++) {
            for (int neighbours : adj.get(i)) {
                indegree[neighbours]++;
            }
        }
        
        // Return the in-degree array.
        return indegree;
    }
}
```


## 19 Course Schedule I and II | Pre-requisite Tasks | Topological Sort
https://www.geeksforgeeks.org/problems/prerequisite-tasks/1
###### Explanation of Steps:

1. **Create Adjacency List:**
    
    - The `getAdjacencyList` function creates an adjacency list representation of the graph from the given edge list (prerequisites).
2. **Compute In-degrees:**
    
    - The `getIndegrees` function computes the in-degrees of all vertices. In-degree of a vertex is the number of edges directed towards it.
3. **Initialize Queue:**
    
    - Initialize a queue and enqueue all vertices with in-degree 0. These vertices can be processed first in the topological order.
4. **Process Vertices in Topological Order:**
    
    - Dequeue a vertex from the queue, reduce the in-degrees of its adjacent vertices, and enqueue those vertices whose in-degrees become 0. Keep track of the count of processed vertices.
5. **Check if All Vertices are Processed:**
    
    - If the count of processed vertices is equal to the number of vertices (`V`), it indicates that all vertices were processed successfully, and there are no cycles. Hence, return `true`. Otherwise, return `false`.

```java
// User function Template for Java

class Solution {
    public boolean isPossible(int V, int P, int[][] prerequisites) {
        // Step 1: Create adjacency list representation of the graph from prerequisites
        List<List<Integer>> adj = getAdjacencyList(prerequisites, V);
        
        // Step 2: Compute in-degrees of all vertices
        int[] indegree = getIndegrees(adj, V);
        
        // Step 3: Initialize queue and enqueue all vertices with in-degree 0
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }
        
        // Step 4: Process the vertices in topological order
        int cnt = 0;
        while (!q.isEmpty()) {
            int node = q.poll();
            cnt++;
            
            // Reduce in-degree of adjacent vertices and add to queue if in-degree becomes 0
            for (int nbh : adj.get(node)) {
                indegree[nbh]--;
                if (indegree[nbh] == 0) {
                    q.offer(nbh);
                }
            }
        }
        
        // Step 5: If count of processed vertices is equal to V, return true, else return false
        return cnt == V;
    }
    
    // Function to compute in-degrees of all vertices
    int[] getIndegrees(List<List<Integer>> adj, int V) {
        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int neighbour : adj.get(i)) {
                indegree[neighbour]++;
            }
        }
        return indegree;
    }
    
    // Function to create adjacency list from edge list
    List<List<Integer>> getAdjacencyList(int[][] arr, int V) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Add edges to the adjacency list
        for (int i = 0; i < arr.length; i++) {
            adj.get(arr[i][0]).add(arr[i][1]);
        }
        return adj;
    }
}

```

https://leetcode.com/problems/course-schedule/description/

```java
class Solution {
    public boolean canFinish(int V, int[][] prerequisites) {
    // Step 1: Create adjacency list representation of the graph from prerequisites
        List<List<Integer>> adj = getAdjacencyList(prerequisites, V);
        
        // Step 2: Compute in-degrees of all vertices
        int[] indegree = getIndegrees(adj, V);
        
        // Step 3: Initialize queue and enqueue all vertices with in-degree 0
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }
        
        // Step 4: Process the vertices in topological order
        int cnt = 0;
        while (!q.isEmpty()) {
            int node = q.poll();
            cnt++;
            
            // Reduce in-degree of adjacent vertices and add to queue if in-degree becomes 0
            for (int nbh : adj.get(node)) {
                indegree[nbh]--;
                if (indegree[nbh] == 0) {
                    q.offer(nbh);
                }
            }
        }
        
        // Step 5: If count of processed vertices is equal to V, return true, else return false
        return cnt == V;
    }
    
    // Function to compute in-degrees of all vertices
    int[] getIndegrees(List<List<Integer>> adj, int V) {
        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int neighbour : adj.get(i)) {
                indegree[neighbour]++;
            }
        }
        return indegree;
    }
    
    // Function to create adjacency list from edge list
    List<List<Integer>> getAdjacencyList(int[][] arr, int V) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Add edges to the adjacency list
        for (int i = 0; i < arr.length; i++) {
            adj.get(arr[i][0]).add(arr[i][1]);
        }
        return adj;
    }
}
```

https://leetcode.com/problems/course-schedule-ii/description/

```java
class Solution {
    public int[] findOrder(int V, int[][] prerequisites) {
     // Step 1: Create adjacency list representation of the graph from prerequisites
        List<List<Integer>> adj = getAdjacencyList(prerequisites, V);
        
        // Step 2: Compute in-degrees of all vertices
        int[] indegree = getIndegrees(adj, V);
        
        // Step 3: Initialize queue and enqueue all vertices with in-degree 0
        Queue<Integer> q = new LinkedList<>();
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }
        
        // Step 4: Process the vertices in topological order
        int i = 0;
        int[] res = new int[V];
        while (!q.isEmpty()) {
            int node = q.poll();
            res[i++] = node;
            
            // Reduce in-degree of adjacent vertices and add to queue if in-degree becomes 0
            for (int nbh : adj.get(node)) {
                indegree[nbh]--;
                if (indegree[nbh] == 0) {
                    q.offer(nbh);
                }
            }
        }
        
        // Step 5: If count of processed vertices is equal to V, return true, else return false
        return (i == V) ? res : new int[]{};
    }
    
    // Function to compute in-degrees of all vertices
    int[] getIndegrees(List<List<Integer>> adj, int V) {
        int[] indegree = new int[V];
        for (int i = 0; i < V; i++) {
            for (int neighbour : adj.get(i)) {
                indegree[neighbour]++;
            }
        }
        return indegree;
    }
    
    // Function to create adjacency list from edge list
    List<List<Integer>> getAdjacencyList(int[][] arr, int V) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Add edges to the adjacency list
        for (int i = 0; i < arr.length; i++) {
            adj.get(arr[i][1]).add(arr[i][0]);
        }
        return adj;
    }
}
```

## 20 [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/) | Using Topo Sort (BFS)

###### Explanation:

1. **Reverse the Graph**: The `getAdjMat` function creates a reversed version of the input graph. In the original graph, if there is an edge from `u` to `v`, in the reversed graph, there will be an edge from `v` to `u`.

2. **Compute Indegrees**: The `getIndegrees` function calculates the indegree (the number of incoming edges) for each node in the reversed graph.

3. **Initialize Queue**: All nodes with 0 indegree (i.e., no incoming edges) are added to the queue. These nodes are considered safe because they don't depend on any other nodes.

4. **Topological Sort**: Using a queue, perform a topological sort. For each node removed from the queue, it's added to the list of safe nodes. Then, the indegree of all its neighbors is reduced by 1. If any neighbor's indegree becomes 0, it's added to the queue.

5. **Sort and Return**: Finally, the list of safe nodes is sorted and returned.

By following these steps, the function identifies all nodes that are eventually safe (i.e., nodes that do not participate in any cycles).

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        int V = graph.length; // Number of vertices in the graph

        // Step 1: Reverse the edges of the graph
        List<List<Integer>> adj = getAdjMat(graph);

        // Step 2: Compute the indegree of each node in the reversed graph
        int[] indegree = getIndegrees(adj);

        Queue<Integer> q = new LinkedList<>();
        List<Integer> safeState = new ArrayList<>();

        // Step 3: Initialize the queue with all nodes having 0 indegree
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }

        // Step 4: Perform topological sort
        while (!q.isEmpty()) {
            int node = q.poll(); // Remove a node with 0 indegree
            safeState.add(node); // Add it to the list of safe nodes

            // Reduce the indegree of all its neighbors
            for (int nbh : adj.get(node)) {
                indegree[nbh]--;
                // If indegree of neighbor becomes 0, add it to the queue
                if (indegree[nbh] == 0) {
                    q.offer(nbh);
                }
            }
        }

        // Step 5: Sort the list of safe nodes and return
        Collections.sort(safeState);
        return safeState;
    }

    // Function to reverse the edges of the graph
    private List<List<Integer>> getAdjMat(int[][] graph) {
        int V = graph.length;
        List<List<Integer>> adj = new ArrayList<>();

        // Initialize adjacency list
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }

        // Reverse the edges
        for (int i = 0; i < graph.length; i++) {
            for (int next : graph[i]) {
                adj.get(next).add(i);
            }
        }

        return adj;
    }

    // Function to compute the indegree of each node
    private int[] getIndegrees(List<List<Integer>> adj) {
        int V = adj.size();
        int[] indegree = new int[V];

        // Compute indegree of each node
        for (int i = 0; i < V; i++) {
            for (int nbh : adj.get(i)) {
                indegree[nbh]++;
            }
        }

        return indegree;
    }
}
```

## 21 Alien Dictionary
https://www.geeksforgeeks.org/problems/alien-dictionary/1
###### Explanation:

1. **Initialize Adjacency List**: The `solve` function initializes an adjacency list to represent the graph of character dependencies.

2. **Build the Graph**: By comparing each pair of adjacent words in the dictionary, the code determines the order of characters and builds the graph. If the characters at a specific position are different, an edge is added from the character in the first word to the character in the second word.

3. **Topological Sort**: The `topoSort` function performs a topological sort on the graph. Nodes with 0 in-degree (no dependencies) are added to a queue. As each node is processed, its neighbors' in-degrees are decreased, and any neighbor with 0 in-degree is added to the queue.

4. **Convert to String**: The topological order of characters is converted to a string, representing the order of characters in the alien language.

5. **Compute In-Degrees**: The `getIndegrees` function calculates the in-degree for each node in the graph, representing the number of incoming edges for each character.

By following these steps, the code determines the order of characters in the alien language based on the given dictionary.

```java
// User function template for Java

class Solution {
    public String findOrder(String[] dict, int N, int K) {
        // This function finds the order of characters in an alien language
        return solve(dict, N, K);
    }

    // Helper function to solve the problem
    String solve(String[] dict, int N, int K) {
        // Step 1: Create an adjacency list for the graph
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < K; i++) {
            adj.add(new ArrayList<>());
        }

        // Step 2: Build the graph by comparing adjacent words
        for (int i = 0; i < N - 1; i++) {
            String s1 = dict[i];
            String s2 = dict[i + 1];
            int len = Math.min(s1.length(), s2.length());

            // Compare characters of both words
            for (int ptr = 0; ptr < len; ptr++) {
                if (s1.charAt(ptr) != s2.charAt(ptr)) {
                    // Add an edge from s1[ptr] to s2[ptr]
                    adj.get(s1.charAt(ptr) - 'a').add(s2.charAt(ptr) - 'a');
                    break;
                }
            }
        }

        // Step 3: Perform topological sort on the graph
        List<Integer> topo = topoSort(K, adj);

        // Step 4: Convert the topological order to a string
        String ans = "";
        for (int it : topo) {
            ans += (char) (it + (int) ('a'));
        }
        return ans;
    }

    // Function to perform topological sort on the graph
    private List<Integer> topoSort(int V, List<List<Integer>> adj) {
        // Step 1: Compute in-degrees of all vertices
        int[] indegree = getIndegrees(adj, V);
        Queue<Integer> q = new LinkedList<>();

        // Step 2: Initialize the queue with all nodes having 0 in-degree
        for (int i = 0; i < V; i++) {
            if (indegree[i] == 0) {
                q.offer(i);
            }
        }

        // Step 3: Process nodes in topological order
        List<Integer> res = new ArrayList<>();
        while (!q.isEmpty()) {
            int node = q.poll(); // Remove a node with 0 in-degree
            res.add(node); // Add it to the topological order

            // Reduce the in-degree of all its neighbors
            for (int nbh : adj.get(node)) {
                indegree[nbh]--;
                // If in-degree of neighbor becomes 0, add it to the queue
                if (indegree[nbh] == 0) {
                    q.offer(nbh);
                }
            }
        }
        return res;
    }

    // Function to compute in-degrees of all vertices
    int[] getIndegrees(List<List<Integer>> adj, int V) {
        int[] indegree = new int[V];
        // Compute in-degree of each node
        for (int i = 0; i < V; i++) {
            for (int neighbour : adj.get(i)) {
                indegree[neighbour]++;
            }
        }
        return indegree;
    }
}
```


## 21 Shortest path in Directed Acyclic Graph
[Shortest path in Directed Acyclic Graph | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph/0)
###### Explanation:

1. **Adjacency List Creation (`getAdjList`)**:
   - Converts the edge list into an adjacency list representation. Each node has a list of pairs representing the connected nodes and the edge weights.

2. **Topological Sort (`getTopoSort` and `topoSortDFS`)**:
   - Performs a DFS-based topological sort. Nodes are processed and pushed onto a stack in topological order.

3. **Shortest Path Calculation (`shortestPath`)**:
   - Initializes the distance array with a large value (`Integer.MAX_VALUE`), representing infinity, and sets the distance to the source node (assumed to be 0) as 0.
   - Processes nodes in topological order, updating the distance to each node's neighbors if a shorter path is found.
   - Finally, replaces distances that are still `Integer.MAX_VALUE` (unreachable nodes) with -1.


```java
// User function Template for Java
import java.util.*;

class Solution {
    // Function to find the shortest path in a DAG using Topological Sort
    public int[] shortestPath(int N, int M, int[][] edges) {
        // Step 1: Create adjacency list from edges
        List<List<Pair>> adj = getAdjList(N, edges);

        // Step 2: Perform topological sort using DFS
        Stack<Integer> topoStack = getTopoSort(N, adj);

        // Step 3: Initialize distance array with a large value (representing infinity)
        int[] dist = new int[N];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[0] = 0;  // Assuming 0 is the source node

        // Step 4: Process nodes in topological order
        while (!topoStack.isEmpty()) {
            int node = topoStack.pop();

            // Update distances for adjacent nodes
            for (Pair neighbor : adj.get(node)) {
                int v = neighbor.vertex;
                int wt = neighbor.weight;

                if (dist[node] != Integer.MAX_VALUE && dist[node] + wt < dist[v]) {
                    dist[v] = dist[node] + wt;
                }
            }
        }

        // Step 5: Replace distances that are still infinity with -1
        for (int i = 0; i < N; i++) {
            if (dist[i] == Integer.MAX_VALUE) {
                dist[i] = -1;
            }
        }

        return dist;
    }

    // Function to create an adjacency list from the given edges
    private List<List<Pair>> getAdjList(int N, int[][] edges) {
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < N; i++) {
            adj.add(new ArrayList<>());
        }

        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int wt = edge[2];
            adj.get(u).add(new Pair(v, wt));
        }
        return adj;
    }

    // Function to perform topological sort using DFS
    private Stack<Integer> getTopoSort(int N, List<List<Pair>> adj) {
        boolean[] visited = new boolean[N];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < N; i++) {
            if (!visited[i]) {
                topoSortDFS(i, adj, visited, stack);
            }
        }
        return stack;
    }

    // DFS helper function for topological sorting
    private void topoSortDFS(int node, List<List<Pair>> adj, boolean[] visited, Stack<Integer> stack) {
        visited[node] = true;
        for (Pair neighbor : adj.get(node)) {
            if (!visited[neighbor.vertex]) {
                topoSortDFS(neighbor.vertex, adj, visited, stack);
            }
        }
        stack.push(node);
    }

    // Pair class to store vertex and weight
    class Pair {
        int vertex, weight;

        Pair(int vertex, int weight) {
            this.vertex = vertex;
            this.weight = weight;
        }
    }
}
```


## 22 Shortest path in Undirected Graph | BFS
https://www.geeksforgeeks.org/problems/shortest-path-in-undirected-graph-having-unit-distance/1
###### Steps:
1. **Create Adjacency List**: Convert the edge list to an adjacency list for efficient graph representation.
2. **Initialize Distance Array**: Set the initial distances to infinity (represented by `Integer.MAX_VALUE`).
3. **Set Source Distance**: The distance from the source node to itself is zero.
4. **Initialize Queue for BFS**: Use a queue to perform a Breadth-First Search (BFS) starting from the source node.
5. **Perform BFS**: For each node, update the distances to its neighbors if a shorter path is found.
6. **Handle Unreachable Nodes**: Replace distances of nodes that remain unreachable (`Integer.MAX_VALUE`) with `-1`.
7. **Return Result**: Return the array of shortest distances from the source node to all other nodes.

```java
import java.util.*;

class Solution {

    // Function to find the shortest path from a source node to all other nodes in an undirected graph
    public int[] shortestPath(int[][] edges, int n, int m, int src) {
        // Step 1: Create adjacency list from edge list
        List<List<Integer>> adj = getAdjList(edges, n, m);

        // Step 2: Initialize distance array with infinity
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        
        // Step 3: Set distance to source node as 0
        dist[src] = 0;

        // Step 4: Initialize queue for BFS
        Queue<Integer> q = new LinkedList<>();
        q.offer(src);

        // Step 5: Perform BFS to find shortest path
        while (!q.isEmpty()) {
            int node = q.poll();
            for (int neighbor : adj.get(node)) {
                if (dist[node] + 1 < dist[neighbor]) {
                    dist[neighbor] = dist[node] + 1;
                    q.add(neighbor);
                }
            }
        }

        // Step 6: Replace distances of unreachable nodes with -1
        for (int i = 0; i < n; i++) {
            if (dist[i] == Integer.MAX_VALUE) {
                dist[i] = -1;
            }
        }

        // Step 7: Return the distance array
        return dist;
    }

    // Helper function to create an adjacency list from the edge list
    private List<List<Integer>> getAdjList(int[][] edges, int n, int m) {
        List<List<Integer>> adj = new ArrayList<>();

        // Initialize adjacency list
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Populate adjacency list
        for (int i = 0; i < m; i++) {
            adj.get(edges[i][0]).add(edges[i][1]);
            adj.get(edges[i][1]).add(edges[i][0]);
        }

        return adj;
    }
}
```

## 23 [Word Ladder](https://leetcode.com/problems/word-ladder/)

###### Steps Explained

1. **Class and Method Definition**:
   - Define the `Solution` class.
   - The `ladderLength` method is the entry point that calls the private `solve` method.

2. **Initialization**:
   - Initialize a queue `q` to store pairs of words and their respective transformation steps.
   - Add the initial `beginWord` with step count 1 to the queue.
   - Create a set `set` from the `wordList` for quick lookup of words and remove the `beginWord` from it.

3. **Breadth-First Search (BFS)**:
   - While the queue is not empty:
     - Dequeue the first element and get the current word and its step count.
     - If the current word matches the `endWord`, return the step count.
     - For each character in the current word, try replacing it with every letter from 'a' to 'z' to form new words.
     - If a newly formed word is in the set, remove it from the set and add it to the queue with incremented step count.

4. **Return Statement**:
   - If no transformation sequence leads to the `endWord`, return 0.

5. **Helper Class `Pair`**:
   - A simple class to store pairs of words and their step counts.

This code uses a BFS approach to find the shortest transformation sequence from `beginWord` to `endWord` where each transformed word must be in the `wordList`.

```java
import java.util.*;

// Main class containing the solution
class Solution {
    // Entry point of the solution
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        return solve(beginWord, endWord, wordList);
    }

    // Private method to solve the problem
    private int solve(String beginWord, String endWord, List<String> wordList) {
        // Queue to hold pairs of (current word, number of transformation steps)
        Queue<Pair> q = new LinkedList<>();
        // Start with the beginWord and step count as 1
        q.offer(new Pair(beginWord, 1));

        // Set to store the wordList for quick lookup and remove the beginWord
        Set<String> set = new HashSet<>();
        for (String st : wordList) {
            set.add(st);
        }
        set.remove(beginWord);

        // Perform BFS
        while (!q.isEmpty()) {
            // Get the current word and step count
            String word = q.peek().first;
            int steps = q.peek().second;
            q.poll();

            // If the current word matches the endWord, return the step count
            if (word.equals(endWord)) return steps;

            // Try changing each character in the current word to find new words
            for (int i = 0; i < word.length(); i++) {
                for (char ch = 'a'; ch <= 'z'; ch++) {
                    // Create a new word by changing one character at position i
                    char[] replacedCharArray = word.toCharArray();
                    replacedCharArray[i] = ch;
                    String newWord = new String(replacedCharArray);

                    // If the new word is in the set, add it to the queue with incremented step count
                    if (set.contains(newWord)) {
                        set.remove(newWord);
                        q.offer(new Pair(newWord, steps + 1));
                    }
                }
            }
        }

        // If no transformation sequence is found, return 0
        return 0;
    }

    // Helper class to store pairs of (word, step count)
    class Pair {
        String first;
        int second;

        Pair(String _first, int _second) {
            this.first = _first;
            this.second = _second;
        }
    }
}
```


## 24 Dijkstra Algorithm - Using Priority Queue
[Dijkstra Algorithm | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)

###### Steps Explained:
1. **dijkstra Function**:
    - Calls the utility function `dijkstraUtil` to compute the shortest paths.
2. **dijkstraUtil Function**:
    - Initializes a priority queue to keep track of nodes to be processed, prioritizing those with the smallest known distance.
    - Initializes an array `dist` to store the shortest distance from the source to each node, setting all distances to infinity except for the source node, which is set to 0.
    - Adds the source node to the priority queue.
    - Processes nodes from the priority queue until it is empty:
        - Retrieves the node with the smallest distance.
        - Updates the distances to its adjacent nodes if a shorter path is found.
        - Adds the updated adjacent nodes to the priority queue.
    - Returns the final distances array.
3. **Pair Class**:
    
    - A helper class to store pairs of (distance, node) for use in the priority queue.

```java
// User function Template for Java

class Solution {
    // Function to find the shortest distance of all the vertices from the source vertex S.
    static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        // Call the utility function that implements Dijkstra's algorithm
        return dijkstraUtil(V, adj, S);
    }

    private static int[] dijkstraUtil(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        // Priority queue to store (distance, node) pairs. The priority queue is used to get the node with the smallest distance first.
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.wt - b.wt);
        
        // Initialize distance array with infinity. This array will hold the shortest distance from the source to each node.
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0; // Distance to the source itself is 0.
        
        // Add the source node to the priority queue with a distance of 0.
        pq.offer(new Pair(0, S));
        
        // Continue processing nodes until the priority queue is empty.
        while (!pq.isEmpty()) {
            // Get the node with the smallest distance from the priority queue.
            Pair current = pq.poll();
            int dis = current.wt; // Current distance from the source.
            int node = current.node; // Current node index.

            // Process all adjacent nodes of the current node.
            for (List<Integer> nb : adj.get(node)) {
                int adjNode = nb.get(0); // Adjacent node index.
                int edgeWeight = nb.get(1); // Weight of the edge to the adjacent node.
                
                // If the new calculated distance to the adjacent node is smaller than the known distance, update it.
                if (dis + edgeWeight					
					< dist[adjNode]) {
                    dist[adjNode] = dis + edgeWeight; // Update the distance.
                    pq.offer(new Pair(dist[adjNode], adjNode)); // Add the adjacent node to the priority queue.
                }
            }
        }
        
        // Return the final distances array, which contains the shortest distances from the source to all other nodes.
        return dist;
    }

    // Pair class to store distance and node. This is used for the priority queue.
    static class Pair {
        int wt;   // Distance from source.
        int node; // Node index.
        
        Pair(int d, int n) {
            wt = d;
            node = n;
        }
    }
}

```

## 25 [Network Delay Time](https://leetcode.com/problems/network-delay-time/)

###### Explanation:

1. **Adjacency List Construction (`getAdjList` method)**:
    
    - This method constructs an adjacency list representation of the graph from the given edges.
    - The adjacency list is initialized with empty lists for each node.
    - For each edge in `times`, it adds a `Pair` (containing the weight and destination node) to the corresponding source node's list.
2. **Dijkstra's Algorithm (`dijkstraUtil` method)**:
    
    - This method uses Dijkstra's algorithm to find the shortest paths from the source node `src` to all other nodes.
    - A priority queue (`pq`) is used to process nodes in order of their current shortest known distance.
    - The `dist` array is initialized with `Integer.MAX_VALUE` to represent infinity, and the distance to the source node is set to 0.
    - The algorithm processes each node, updating the distances to its adjacent nodes if a shorter path is found, and adds these adjacent nodes to the priority queue.
    - After processing all nodes, the method checks if all nodes are reachable. If any node has a distance of `Integer.MAX_VALUE`, it means the node is not reachable, and the method returns -1.
    - Otherwise, it returns the maximum distance found in the `dist` array, which represents the minimum time for all nodes to receive the signal.
3. **Pair Class**:
    
    - The `Pair` class is a simple data structure used to store the weight (`wt`) and node (`node`) for the priority queue and adjacency list.

###### Additional Notes:

- The `dist` array has a size of `V + 1` to accommodate the 1-based indexing of the nodes.
- The priority queue ensures that the node with the smallest current known distance is processed first, which is the key feature of Dijkstra's algorithm.

```java
import java.util.*;

class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        // Construct the adjacency list from the input times
        List<List<Pair>> adj = getAdjList(times, n);
        // Use Dijkstra's algorithm to find the minimum time for all nodes to receive the signal
        return dijkstraUtil(adj, n, k);
    }

    private int dijkstraUtil(List<List<Pair>> adj, int V, int src) {
        // Priority queue to store (distance, node) pairs, ordered by distance
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.wt - b.wt);
        // Distance array to store the shortest distance from the source to each node
        int[] dist = new int[V + 1];
        Arrays.fill(dist, Integer.MAX_VALUE); // Initialize distances to infinity

        dist[src] = 0; // Distance to the source itself is 0
        pq.offer(new Pair(0, src)); // Add the source node to the priority queue

        while (!pq.isEmpty()) {
            Pair curr = pq.poll(); // Get the node with the smallest distance
            int node = curr.node;
            int dis = curr.wt;

            // Process all adjacent nodes
            for (Pair nbs : adj.get(node)) {
                int adjNode = nbs.node;
                int edgeWt = nbs.wt;

                // Check if the new distance is smaller
                if (edgeWt + dis < dist[adjNode]) {
                    dist[adjNode] = edgeWt + dis; // Update the distance
                    pq.offer(new Pair(dist[adjNode], adjNode)); // Add the adjacent node to the priority queue
                }
            }
        }

        // Find the maximum distance to determine the minimum time for all nodes to receive the signal
        int mx = Integer.MIN_VALUE;
        for (int i = 1; i <= V; i++) {
            if (dist[i] == Integer.MAX_VALUE) return -1; // If any node is unreachable, return -1
            mx = Math.max(mx, dist[i]); // Track the maximum distance
        }
        return mx; // Return the maximum distance as the result
    }

    private List<List<Pair>> getAdjList(int[][] times, int V) {
        List<List<Pair>> adj = new ArrayList<>();

        // Initialize the adjacency list with empty lists
        for (int i = 0; i <= V; i++) {
            adj.add(new ArrayList<>());
        }
        // Fill the adjacency list with the given edges and their weights
        for (int[] time : times) {
            int u = time[0];
            int v = time[1];
            int wt = time[2];
            adj.get(u).add(new Pair(wt, v));
        }
        return adj; // Return the constructed adjacency list
    }

    class Pair {
        int node;
        int wt;
        
        Pair(int w, int n) {
            node = n;
            wt = w;
        }
    }
}
```

## 26 Dijkstra Using TreeSet
[Dijkstra Algorithm | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)

```java


//User function Template for Java



class Solution {
    // Function to find the shortest distance of all the vertices
    // from the source vertex S.
    static int[] dijkstra(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        // Call the utility function
        return dijkstraUtil(V, adj, S);
    }

    private static int[] dijkstraUtil(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj, int S) {
        // Priority queue to store (distance, node) pairs
        // PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.wt - b.wt);
        TreeSet<Pair> st = new TreeSet<>((a, b) -> {
            if (a.wt != b.wt) {
                return Integer.compare(a.wt, b.wt);
            }
            return Integer.compare(a.node, b.node);
        });
        
        // Initialize distance array with infinity
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[S] = 0;
        
        // Add source node to priority queue
        st.add(new Pair(0, S));
        
        while (!st.isEmpty()) {
            // Get the node with the smallest distance
            Pair current = st.pollFirst();
            int dis = current.wt;
            int node = current.node;

            // Process all adjacent nodes
            for (List<Integer> nb : adj.get(node)) {
                int adjNode = nb.get(0);
                int edgeWeight = nb.get(1);
                
                // Check if the new distance is smaller
                if (dis + edgeWeight < dist[adjNode]) {
                    if (dist[adjNode]!=Integer.MAX_VALUE)
                        st.remove(new Pair(dist[adjNode], adjNode));
                    dist[adjNode] = dis + edgeWeight;
                    st.add(new Pair(dist[adjNode], adjNode));
                }
            }
        }
        
        // Return the final distances
        return dist;
    }

    // Pair class to store distance and node
    static class Pair {
        int wt;   // distance from source
        int node; // node index
        
        Pair(int d, int n) {
            wt = d;
            node = n;
        }
    }
}

```


## 27 Print Shortest Path - Dijkstra's Algorithm

- You need to implement the caching here | keep the track of where am I coming from
###### Detailed Explanation of Each Step

1. **Wrapper Method (`shortestPath`)**:
   - This method is a simple wrapper that calls the `solve` method to compute the shortest path.

2. **Solve Method**:
   - **Adjacency List Generation (`getAdjList`)**: Convert the edge list to an adjacency list representation.
   - **Priority Queue Initialization**: Use a min-heap priority queue to always process the node with the smallest known distance.
   - **Distance and Parent Array Initialization**: 
     - `dist` array stores the shortest distance from the start node to each node.
     - `parent` array is used to reconstruct the path.
   - **Starting Node Setup**: Initialize the distance for the starting node (node 0) to `0` and set its parent to itself.
   - **Processing the Queue**: Use Dijkstra's algorithm to update the shortest paths:
     - Extract the node with the smallest distance.
     - For each adjacent node, if a shorter path is found, update the distance and parent, and push the adjacent node into the priority queue.
   - **Path Reconstruction**: 
     - If there's no path to the target node, return `-1`.
     - Otherwise, trace back from the target node using the `parent` array to reconstruct the path, converting indices back to 1-based.
     - Reverse the path to get it in the correct order from start to end.

3. **Adjacency List Method (`getAdjList`)**:
   - Initializes the adjacency list with empty lists.
   - Populates the adjacency list with pairs of (weight, adjacent node) for each edge, converting from 1-based to 0-based indices.

4. **Pair Class**:
   - A simple class to store a pair of integers representing a node and its associated weight. Used in the adjacency list and priority queue.
```java
import java.util.*;

class Solution {
    public List<Integer> shortestPath(int n, int m, int edges[][]) {
        // Wrapper method that calls the solve method to get the shortest path
        return solve(n, m, edges);
    }

    private List<Integer> solve(int n, int m, int[][] edges) {
        // Generate adjacency list from the given edges
        List<List<Pair>> adj = getAdjList(n, m, edges);
        
        // Priority queue for Dijkstra's algorithm (min-heap based on weight)
        PriorityQueue<Pair> q = new PriorityQueue<>((a, b) -> a.wt - b.wt);
        
        // Initialize distances to infinity
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        
        // Initialize parent array to keep track of the path
        int[] parent = new int[n];
        Arrays.fill(parent, -1);  // Use -1 to indicate no parent initially
        
        // Start from node 0 (corresponds to node 1 in 1-based indexing)
        q.offer(new Pair(0, 0)); // Start with node 0 and distance 0
        dist[0] = 0;
        parent[0] = 0;  // The parent of the start node is itself

        // Process the priority queue until it's empty
        while (!q.isEmpty()) {
            Pair curr = q.poll();
            int node = curr.node;
            int dis = curr.wt;
            
            // Explore all adjacent nodes
            for (Pair p : adj.get(node)) {
                int adjNode = p.node;
                int wt = p.wt;
                
                // If a shorter path is found, update the distance and parent arrays
                if (dis + wt < dist[adjNode]) {
                    dist[adjNode] = dis + wt;
                    q.offer(new Pair(dist[adjNode], adjNode));
                    parent[adjNode] = node; // Set the parent for path reconstruction
                }
            }
        }

        // Prepare the path
        List<Integer> path = new ArrayList<>();
        if (dist[n - 1] == Integer.MAX_VALUE) { // If there's no path to the target node
            path.add(-1); // Return -1 indicating no path
            return path;
        }
        
        // Reconstruct the path from the parent array
        int node = n - 1; // Start from the last node (n - 1)
        while (parent[node] != node) { // Traverse until the start node
            path.add(node + 1); // Convert back to 1-based index
            node = parent[node];
        }
        path.add(1); // Add the start node (1-based index)
        Collections.reverse(path); // Reverse to get the correct order
        System.out.println(path);
        return path;
    }

    private List<List<Pair>> getAdjList(int n, int m, int[][] edges) {
        // Initialize adjacency list
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        // Populate adjacency list from edges
        for (int[] g : edges) {
            adj.get(g[0] - 1).add(new Pair(g[2], g[1] - 1)); // Convert to zero-based index
            adj.get(g[1] - 1).add(new Pair(g[2], g[0] - 1)); // Convert to zero-based index
        }
        return adj;
    }

    class Pair {
        int node;
        int wt;

        Pair(int wt, int node) {
            this.wt = wt;
            this.node = node;
        }
    }
}
```


## 28 Shortest Distance in a Binary Maze
[Shortest Distance in a Binary Maze | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/shortest-path-in-a-binary-maze-1655453161/1)

###### Explanation

1. **Initialization**:
   - The `shortestPath` method initializes the BFS search by calling the `solve` method.
   - The `solve` method checks if the source is the same as the destination and returns `0` if true.
   - It initializes a queue for BFS and a 2D array `dist` to store the minimum distance from the source to each cell, initially set to `Integer.MAX_VALUE`.

2. **BFS Implementation**:
   - The source cell distance is set to `0`, and it is added to the queue.
   - Possible movements (up, right, down, left) are defined by `dRow` and `dCol`.

3. **Processing the Queue**:
   - The BFS loop continues until the queue is empty.
   - For each cell, it explores all 4 possible directions.
   - If the new cell is within bounds, is a valid cell (`grid[newRow][newCol] == 1`), and offers a shorter path, the distance is updated, and the new cell is added to the queue.

4. **Checking Destination**:
   - If the destination cell is reached, the method returns the distance.
   - If no valid path exists, the method returns `-1`.

This code ensures an efficient BFS traversal with clear comments explaining each part.

```java
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;

class Solution {

    // Main function to find the shortest path in the grid from source to destination
    int shortestPath(int[][] grid, int[] source, int[] destination) {
        return solve(grid, source, destination);
    }
    
    // Helper function to solve the shortest path problem using BFS
    private int solve(int[][] grid, int[] source, int[] destination) {
        // Base case: if the source is the same as the destination
        if (source[0] == destination[0] && source[1] == destination[1]) {
            return 0;
        }

        // Initialize a queue for BFS
        Queue<Tuple> queue = new LinkedList<>();
        
        int n = grid.length;    // Number of rows
        int m = grid[0].length; // Number of columns
        
        // Initialize the distance array with infinity
        int[][] dist = new int[n][m];
        for (int[] row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        
        // Starting point distance is 0
        dist[source[0]][source[1]] = 0;
        queue.offer(new Tuple(0, source[0], source[1]));
        
        // Possible movements: up, right, down, left
        int[] dRow = {-1, 0, 1, 0};
        int[] dCol = {0, 1, 0, -1};
        
        // BFS loop
        while (!queue.isEmpty()) {
            Tuple current = queue.poll();
            int distance = current.distance;
            int row = current.row;
            int col = current.col;
            
            // Explore all 4 possible directions
            for (int i = 0; i < 4; i++) {
                int newRow = row + dRow[i];
                int newCol = col + dCol[i];
                
                // Check if the new position is within bounds and is a valid cell
                if (newRow >= 0 && newRow < n 
                    && newCol >= 0 && newCol < m 
                    && grid[newRow][newCol] == 1 
                    && distance + 1 < dist[newRow][newCol]) {
                    
                    // Update the distance
                    dist[newRow][newCol] = distance + 1;
                    
                    // If the destination is reached, return the distance
                    if (newRow == destination[0] && newCol == destination[1]) {
                        return distance + 1;
                    }
                    
                    // Add the new position to the queue
                    queue.offer(new Tuple(distance + 1, newRow, newCol));
                }
            }
        }
        
        // Return -1 if there is no valid path
        return -1;
    }

    // Custom tuple class to store the state in the BFS
    class Tuple {
        int distance, row, col;

        Tuple(int distance, int row, int col) {
            this.distance = distance;
            this.row = row;
            this.col = col;
        }
    }
}
```

## 29 Path With Minimum Effort

###### Explanation:

1. **Initialization**:
   - The `MinimumEffort` method initializes the search by calling the `solve` method.
   - The `solve` method initializes a priority queue for Dijkstra's algorithm.
   - The `dist` array stores the minimum effort to reach each cell, initialized to `Integer.MAX_VALUE`.

2. **Starting Point**:
   - The starting cell (0,0) is initialized with zero effort and added to the priority queue.

3. **Priority Queue**:
   - The priority queue is used to always process the cell with the minimum current effort first.

4. **Processing the Queue**:
   - The Dijkstra's algorithm loop continues until the queue is empty.
   - For each cell, it explores all 4 possible directions (up, right, down, left).
   - If the new cell is within bounds, the effort to move to the new cell is calculated.
   - If the new effort is less than the current known effort to reach the new cell, the distance is updated, and the new cell is added to the queue.

5. **Checking Destination**:
   - If the destination cell is reached, the method returns the effort.
   - If no valid path exists, the method returns `0` (though this case should not occur if there's a valid path).

This code ensures an efficient Dijkstra's algorithm traversal with clear comments explaining each part.

```java
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
    // Main function to find the minimum effort path in the grid
    public static int MinimumEffort(int rows, int columns, int[][] heights) {
        return solve(rows, columns, heights);
    }
    
    // Helper function to solve the minimum effort path problem using Dijkstra's algorithm
    private static int solve(int rows, int columns, int[][] heights) {
        // Priority queue to store the cells with their current minimum effort required
        PriorityQueue<Tuple> pq = new PriorityQueue<>((a, b) -> a.distance - b.distance);
        
        int n = heights.length;
        int m = heights[0].length;
        
        // Distance array to store the minimum effort to reach each cell, initialized to infinity
        int[][] dist = new int[n][m];
        for (int[] row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        
        // Starting point has zero effort
        dist[0][0] = 0;
        pq.offer(new Tuple(0, 0, 0));
        
        // Possible movements: up, right, down, left
        int[] dRow = {-1, 0, 1, 0};
        int[] dCol = {0, 1, 0, -1};
        
        // Dijkstra's algorithm to find the minimum effort path
        while (!pq.isEmpty()) {
            Tuple curr = pq.poll();
            int currentEffort = curr.distance;
            int row = curr.row;
            int col = curr.col;
            
            // If we reach the bottom-right corner, return the effort
            if (row == n - 1 && col == m - 1) {
                return currentEffort;
            }
            
            // Explore all 4 possible directions
            for (int i = 0; i < 4; i++) {
                int newRow = row + dRow[i];
                int newCol = col + dCol[i];
                
                // Check if the new position is within bounds
                if (newRow >= 0 && newRow < n && newCol >= 0 && newCol < m) {
                    // Calculate the effort required to move to the new cell
                    int newEffort = Math.max(
                        Math.abs(heights[row][col] - heights[newRow][newCol]),
                        currentEffort
                    );
                    
                    // If the new effort is less than the current known effort to reach the new cell
                    if (newEffort < dist[newRow][newCol]) {
                        // Update the effort and add the new cell to the priority queue
                        dist[newRow][newCol] = newEffort;
                        pq.offer(new Tuple(newEffort, newRow, newCol));
                    }
                }
            }
        }
        
        // Return 0 if there's no valid path (should not reach here if there's a valid path)
        return 0;
    }
    
    // Custom tuple class to store the state in the priority queue
    static class Tuple {
        int distance;
        int row;
        int col;
        
        Tuple(int distance, int row, int col) {
            this.distance = distance;
            this.row = row;
            this.col = col;
        }
    }
}
```


## 30 [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

###### Explanation:

1. **`findCheapestPrice` Method**:
    - This method calculates the cheapest price to travel from the `source` city to the `destination` city within a specified maximum number of stops using a BFS approach.

2. **Initialization**:
    - **Adjacency List**: 
        - The graph is represented using an adjacency list where each city has a list of `Flight` objects representing the outgoing flights.
        - The list is populated using the input `flights` array, which contains details of all available flights between cities.
    - **Queue and Costs Array**:
        - A `Queue<Tuple>` is used to perform BFS. Each `Tuple` stores the number of stops taken so far, the current city, and the cumulative travel cost.
        - The `costs` array is used to keep track of the minimum cost required to reach each city. Initially, all cities are set to be unreachable (`Integer.MAX_VALUE`), except for the source city, which has a cost of `0`.

3. **BFS Exploration**:
    - The BFS loop continues until all possible paths have been explored or the queue is empty.
    - For each city, it checks all its adjacent cities (cities that can be directly reached from the current city).
    - If a cheaper path is found to an adjacent city, the path cost is updated, and the adjacent city is added to the queue for further exploration.
    - The process ensures that the number of stops does not exceed the allowed `maxStops`.

4. **Return Condition**:
    - After BFS completes, the method checks if the `destination` city was reachable within the allowed number of stops. If not, it returns `-1`; otherwise, it returns the minimum cost to reach the `destination` city.

5. **Helper Classes**:
    - **`Flight` Class**: Represents a flight with a destination city (`toCity`) and a price (`price`).
    - **`Tuple` Class**: Represents a state in the BFS with the current city, number of stops taken, and the cumulative travel cost. This helps track the progress during BFS.

```java
class Solution {

    // Function to find the cheapest price to travel from source to destination within a given number of stops
    public int findCheapestPrice(int numOfCities, int[][] flights, int source, int destination, int maxStops) {

        // Create an adjacency list to represent the graph of cities and flights
        ArrayList<ArrayList<Flight>> adjacencyList = new ArrayList<>();
        for (int i = 0; i < numOfCities; i++) {
            adjacencyList.add(new ArrayList<>());  // Initialize a list for each city
        }

        // Populate the adjacency list with flight information
        for (int[] flight : flights) {
            int fromCity = flight[0];
            int toCity = flight[1];
            int price = flight[2];
            adjacencyList.get(fromCity).add(new Flight(toCity, price));  // Add flight details to the corresponding city's list
        }

        // Queue to perform BFS, each tuple contains (stops, current city, current cost)
        Queue<Tuple> queue = new LinkedList<>();
        queue.add(new Tuple(0, source, 0));  // Start from the source city with 0 stops and 0 cost

        // Array to store the minimum cost to reach each city
        int[] costs = new int[numOfCities];
        for (int i = 0; i < numOfCities; i++) {
            costs[i] = Integer.MAX_VALUE;  // Initialize all costs to infinity (unreachable)
        }
        costs[source] = 0;  // Cost to reach the source city is 0

        // Perform BFS to explore the cheapest path
        while (!queue.isEmpty()) {
            Tuple current = queue.poll();
            int stops = current.stops;
            int city = current.city;
            int cost = current.cost;

            // If the number of stops exceeds the allowed maximum, skip further exploration
            if (stops > maxStops) continue;

            // Explore all adjacent cities from the current city
            for (Flight flight : adjacencyList.get(city)) {
                int adjacentCity = flight.toCity;
                int edgeWeight = flight.price;

                // If a cheaper path to the adjacent city is found, update the cost and add the city to the queue
                if (cost + edgeWeight < costs[adjacentCity] && stops <= maxStops) {
                    costs[adjacentCity] = cost + edgeWeight;
                    queue.add(new Tuple(stops + 1, adjacentCity, cost + edgeWeight));
                }
            }
        }

        // If the destination city is unreachable, return -1, otherwise return the minimum cost
        return costs[destination] == Integer.MAX_VALUE ? -1 : costs[destination];
    }
    
    // Helper class to represent a flight from one city to another with a specific price
    class Flight {
        int toCity;
        int price;

        Flight(int toCity, int price) {
            this.toCity = toCity;
            this.price = price;
        }
    }

    // Helper class to represent a tuple of stops, city, and the current travel cost
    class Tuple {
        int stops;
        int city;
        int cost;

        Tuple(int stops, int city, int cost) {
            this.stops = stops;
            this.city = city;
            this.cost = cost;
        }
    }
}
```

## 31  Minimum Multiplications to reach End
[Minimum Multiplications to reach End | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/minimum-multiplications-to-reach-end/0)
###### Explanation:

1. **`minimumMultiplications` Method**:
    - This is the main method that initializes the process. It first checks if the `start` and `end` values are the same, in which case it immediately returns `0` because no operations are needed.
    - If `start` is not equal to `end`, it calls the `solve` method to find the minimum number of multiplications required.

2. **`solve` Method**:
    - **Initialization**: 
        - A `Queue<Pair>` is initialized to perform Breadth-First Search (BFS). Each `Pair` object stores a number (`node`) and the number of steps taken to reach that number (`steps`).
        - An array `dist` of size `100000` is used to keep track of the minimum number of steps required to reach each possible number. It is initialized to `Integer.MAX_VALUE` to represent an initially unreachable state.
        - The starting number's distance (`dist[start]`) is set to `0`.
    - **BFS Process**:
        - The BFS loop runs as long as there are elements in the queue.
        - For each `node` in the queue, the code tries to multiply it by each element in the array `arr`.
        - The resulting number (`num`) is computed using modulo `100000` to ensure it stays within bounds.
        - If the new number can be reached in fewer steps than previously recorded, the distance is updated, and the new `num` is added to the queue for further exploration.
        - If the `num` equals `end`, the BFS halts, and the number of steps is returned.
    - **Return Condition**: 
        - If the queue is exhausted without finding `end`, the function returns `-1`, indicating that it's impossible to reach `end` from `start` using the given multiplications.

3. **`Pair` Class**:
    - This is a simple helper class used to store a `node` and the number of `steps` taken to reach that node. It allows the BFS to efficiently track the current state and progress.

```java
class Solution {

    // Function to calculate the minimum number of multiplications needed to reach 'end' from 'start'
    int minimumMultiplications(int[] arr, int start, int end) {

        // If the start is the same as the end, no multiplication is needed
        if (start == end) return 0;

        // Call the helper function to perform BFS and find the minimum steps
        return solve(arr, start, end);
    }
    
    // Helper function to perform BFS and find the minimum multiplications needed
    private int solve(int[] arr, int start, int end) {
        // Initialize a queue to perform BFS, storing the current number and the number of steps taken
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(start, 0));
        
        // Create an array to store the minimum number of steps to reach each number
        int[] dist = new int[100000];
        Arrays.fill(dist, Integer.MAX_VALUE);  // Initially, set all distances to infinity
        
        dist[start] = 0;  // Distance to reach the start is 0
        int mod = 100000;  // Use modulo operation to keep numbers within bounds
        int n = arr.length;  // Number of elements in the array
        
        // Perform BFS until the queue is empty
        while (!q.isEmpty()) {
            // Get the front element from the queue
            Pair it = q.poll();
            int node = it.node;
            int steps = it.steps;
            
            // Try multiplying the current number by each number in the array
            for (int i = 0; i < n; i++) {
                int num = (arr[i] * node) % mod;  // Calculate the new number after multiplication

                // If the number of steps to reach this number is less than previously recorded
                if (steps + 1 < dist[num]) {
                    dist[num] = steps + 1;  // Update the minimum steps to reach this number
                    
                    // If this number is the target, return the number of steps taken
                    if (num == end) return steps + 1;
                    
                    // Otherwise, add the new number and the updated steps to the queue
                    q.add(new Pair(num, steps + 1));
                }
            }
        }
        // If the end cannot be reached, return -1
        return -1;
    }
    
    // Helper class to store pairs of numbers and their corresponding step count
    class Pair {
        int node;
        int steps;
        Pair(int n, int s) {
            node = n;
            steps = s;
        }
    }
}
```

## 32 [Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

###### Explanation:
- **countPaths Method**: Handles edge cases and initiates the process by calling the `solve` method.
- **solve Method**: Implements Dijkstra's algorithm to find the shortest paths from the source to all other nodes while also counting the number of ways to reach each node with the minimum distance.
- **Priority Queue**: Used to always expand the least costly node first, ensuring the shortest paths are found.
- **Adjacency List**: Represents the graph, where each node has a list of its neighbors and the weights (distances) of the edges connecting them.
- **Pair Class**: Represents a graph node and its associated weight for easy comparison in the priority queue.

This code efficiently calculates the number of ways to reach the destination node using the shortest path, handling large numbers and edge cases.

```java
import java.util.*;

class Solution {
    public int countPaths(int n, int[][] roads) {
        // Base case: If there's only one city, there's exactly one way to reach it.
        if (n == 1) return 1;
        
        // Edge case: If there are no cities or no roads, there's no valid path.
        if (n == 0 || roads.length == 0) return 0;
        
        // Call the helper function to solve the problem using Dijkstra's algorithm.
        return solve(n, roads);
    }

    private int solve(int n, int[][] roads) {
        // Create the adjacency list representation of the graph.
        List<List<Pair>> adj = getAdjList(n, roads);
        
        // Priority Queue (Min-Heap) to get the node with the smallest distance at each step.
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> Long.compare(a.wt, b.wt));
        
        // Array to store the number of ways to reach each node.
        long[] ways = new long[n];
        
        // Array to store the minimum distance to reach each node.
        long[] dist = new long[n];
        
        // Initialize ways and distances. Initially, no paths are known, so set distances to infinity.
        Arrays.fill(ways, 0);
        Arrays.fill(dist, Long.MAX_VALUE);
        
        // Start from node 0 with a distance of 0.
        pq.offer(new Pair(0, 0));
        
        // Number of ways to reach the starting node is 1.
        ways[0] = 1;
        
        // Distance to the starting node is 0.
        dist[0] = 0;
        
        // Define the modulus for the final result as per the problem statement.
        int mod = (int)(1e9 + 7);
        
        // Dijkstra's algorithm main loop
        while (!pq.isEmpty()) {
            // Extract the node with the smallest distance.
            Pair it = pq.poll();
            int node = it.node;
            long wt = it.wt;
            
            // Explore all adjacent nodes.
            for (Pair p : adj.get(node)) {
                int adjNode = p.node;
                long adjWt = p.wt;
                
                // If a shorter path to adjNode is found.
                if (wt + adjWt < dist[adjNode]) {
                    // Update the shortest distance to adjNode.
                    dist[adjNode] = wt + adjWt;
                    
                    // Update the number of ways to reach adjNode.
                    ways[adjNode] = ways[node];
                    
                    // Push the updated path to the priority queue.
                    pq.offer(new Pair(adjNode, dist[adjNode]));
                } 
                // If an alternative path with the same shortest distance is found.
                else if (wt + adjWt == dist[adjNode]) {
                    // Increment the number of ways to reach adjNode by the ways to reach the current node.
                    ways[adjNode] = (ways[adjNode] + ways[node]) % mod;
                }
            }
        }
        
        // Return the number of ways to reach the destination node (n-1), modulo 1e9 + 7.
        return (int)(ways[n-1] % mod);
    }

    // Helper function to build the adjacency list from the roads input.
    private List<List<Pair>> getAdjList(int n, int[][] roads) {
        List<List<Pair>> adj = new ArrayList<>();
        
        // Initialize the adjacency list for each node.
        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }
        
        // Populate the adjacency list with the given roads.
        for (int[] ls : roads) {
            int u = ls[0];
            int v = ls[1];
            int wt = ls[2];
            
            // Since the graph is undirected, add edges in both directions.
            adj.get(u).add(new Pair(v, wt));
            adj.get(v).add(new Pair(u, wt));
        }
        
        // Return the constructed adjacency list.
        return adj;
    }

    // Helper class to represent a node and its associated weight (distance).
    class Pair {
        int node;
        long wt;
        Pair(int n, long w) {
            node = n;
            wt = w;
        }
    }
}
```

## 33 BellMan-ford-Algorithm

Here is the code with comments and explanations added:

```java
class Solution {
    static int[] bellman_ford(int V, ArrayList<ArrayList<Integer>> edges, int S) {
        // Initialize the distance array with a large value (infinity)
        int[] dist = new int[V];
        Arrays.fill(dist, (int)(1e8)); // 1e8 is used as a substitute for infinity
        
        // Set the distance to the source vertex S to 0
        dist[S] = 0;

        // Relax all edges V-1 times
        for (int i = 0; i < V - 1; i++) {
            // Traverse through all edges
            for (List<Integer> g : edges) {
                int from = g.get(0); // Start vertex of the edge
                int to = g.get(1);   // End vertex of the edge
                int wt = g.get(2);   // Weight of the edge
                
                // Relax the edge (from, to) if a shorter path is found
                if (dist[from] != (int)(1e8) && dist[from] + wt < dist[to]) {
                    dist[to] = dist[from] + wt;
                }
            }
        }

        // Check for negative weight cycles
        for (List<Integer> g : edges) {
            int from = g.get(0); // Start vertex of the edge
            int to = g.get(1);   // End vertex of the edge
            int wt = g.get(2);   // Weight of the edge
            
            // If a shorter path is found after V-1 relaxations, there is a negative cycle
            if (dist[from] != (int)(1e8) && dist[from] + wt < dist[to]) {
                return new int[]{-1}; // Return -1 to indicate the presence of a negative cycle
            }
        }

        // Return the array of shortest distances from source S to all vertices
        return dist;
    }
}
```

###### Explanation:
1. **Initialization**: 
   - The distance array `dist` is initialized with a large value (infinity) to represent that initially all vertices are unreachable from the source.
   - The distance to the source vertex `S` is set to 0 because the distance from the source to itself is always zero.

2. **Relaxation**:
   - The algorithm iteratively relaxes all edges `V-1` times. 
   - For each edge, it checks if the current distance to the destination vertex can be improved by taking the current edge. If so, it updates the distance.

3. **Negative Cycle Detection**:
   - After the `V-1` iterations, the algorithm checks for negative weight cycles by trying to relax the edges one more time. 
   - If a shorter path is still found, it means there is a negative weight cycle, and the algorithm returns `-1`.

4. **Result**:
   - If no negative weight cycle is detected, the algorithm returns the array `dist`, which contains the shortest distances from the source vertex to all other vertices.

```java
class Solution {
    static int[] bellman_ford(int V, ArrayList<ArrayList<Integer>> edges, int S) {
        // Initialize the distance array with a large value (infinity)
        int[] dist = new int[V];
        Arrays.fill(dist, (int)(1e8)); // 1e8 is used as a substitute for infinity
        
        // Set the distance to the source vertex S to 0
        dist[S] = 0;

        // Relax all edges V-1 times
        for (int i = 0; i < V - 1; i++) {
            // Traverse through all edges
            for (List<Integer> g : edges) {
                int from = g.get(0); // Start vertex of the edge
                int to = g.get(1);   // End vertex of the edge
                int wt = g.get(2);   // Weight of the edge
                
                // Relax the edge (from, to) if a shorter path is found
                if (dist[from] != (int)(1e8) && dist[from] + wt < dist[to]) {
                    dist[to] = dist[from] + wt;
                }
            }
        }

        // Check for negative weight cycles
        for (List<Integer> g : edges) {
            int from = g.get(0); // Start vertex of the edge
            int to = g.get(1);   // End vertex of the edge
            int wt = g.get(2);   // Weight of the edge
            
            // If a shorter path is found after V-1 relaxations, there is a negative cycle
            if (dist[from] != (int)(1e8) && dist[from] + wt < dist[to]) {
                return new int[]{-1}; // Return -1 to indicate the presence of a negative cycle
            }
        }

        // Return the array of shortest distances from source S to all vertices
        return dist;
    }
}
```

The **Bellman-Ford algorithm** is an algorithm used to find the shortest paths from a single source vertex to all other vertices in a weighted graph. It is particularly useful for graphs that contain negative weight edges, unlike Dijkstra's algorithm, which cannot handle negative weights.

###### Key Features:

1. **Single Source Shortest Path**: Like Dijkstra's algorithm, Bellman-Ford computes the shortest paths from a single source vertex to all other vertices in the graph.
    
2. **Handles Negative Weights**: The Bellman-Ford algorithm can handle graphs with negative weight edges. However, it also detects if there is a negative weight cycle in the graph, which makes the concept of the "shortest path" undefined, as one could keep reducing the path length by looping through the negative cycle.
    
3. **Time Complexity**: The time complexity of the Bellman-Ford algorithm is O(V×E)O(V \times E)O(V×E), where VVV is the number of vertices and EEE is the number of edges. This makes it less efficient than Dijkstra's algorithm for graphs without negative weights, but it's more versatile.
###### How It Works:

1. **Initialization**:
    
    - Initialize the distance to the source vertex to 0 and to all other vertices as infinity.
2. **Relaxation**:
    
    - The algorithm performs V−1V-1V−1 iterations over all edges, where VVV is the number of vertices.
    - In each iteration, it checks if the distance to the destination vertex of an edge can be reduced by going through the source vertex of the edge. If it can, it updates the distance.
3. **Negative Weight Cycle Detection**:
    
    - After V−1V-1V−1 iterations, the algorithm checks all edges one more time. If any distance can still be reduced, it indicates the presence of a negative weight cycle.