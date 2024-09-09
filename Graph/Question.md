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



## 34. Floyd Warshal Algorithm

- Shortest path  algorihtm
- Multiple source - Multi source shortest path
- Helps to detect negative cycles as well
###### Explanation of the Steps:
1. **Matrix Initialization**: The matrix is first initialized by setting distances between nodes without a direct path to a large value (representing infinity). Distances from any node to itself are set to 0.

2. **Floyd-Warshall Algorithm**: The algorithm is applied to find the shortest path between all pairs of nodes. It iteratively checks if using an intermediate node (`via`) offers a shorter path between any two nodes (`i` and `j`) than the previously known shortest path.

3. **Post-processing**: After computing the shortest paths, the matrix is checked again, and any distance that remains set to the large value (indicating no path exists) is reset to -1 to signify that no path is possible between those nodes.

```java
class Solution
{
    public void shortest_distance(int[][] matrix)
    {
        // Step 1: Initialize the matrix
        int n = matrix.length;

        // Step 2: Preprocess the matrix
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                // If there's no direct path between nodes i and j, set it to a large value (infinity)
                if (matrix[i][j] == -1)  
                    matrix[i][j] = (int)(1e9);

                // The distance from a node to itself should be 0
                if (i == j) 
                    matrix[i][j] = 0;       
            }
        }

        // Step 3: Floyd-Warshall Algorithm
        // Iterate over each possible intermediate node 'via'
        for (int via = 0; via < n; via++)
        {
            // For each pair of nodes (i, j), update the shortest path considering the intermediate node 'via'
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    // Update the distance between node i and node j
                    matrix[i][j] = Math.min
                    (
                        matrix[i][j], // The current shortest distance between i and j
                        matrix[i][via] + matrix[via][j] // The distance through the intermediate node 'via'
                    );
                }
            }
        }

        // Step 4: Post-process the matrix
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                // If the distance is still set to infinity, it means there's no path, so reset it to -1
                if (matrix[i][j] == (int)(1e9))  
                    matrix[i][j] = -1;
            }
        }
    }
}
```

#### If it contains a -ve cycle then small modification in the existing Floyd-Warshall code

To handle negative cycles in the graph when using the Floyd-Warshall algorithm, we need to make a small modification to the code. Specifically, after the main loop of the algorithm (where we update the shortest paths), we should check if any node can reach itself with a negative distance. If such a condition exists, it indicates a negative cycle, and we can mark all nodes involved in that cycle accordingly.
###### Explanation of the Modifications:
1. **Checking for Negative Cycles**:
   - After running the Floyd-Warshall algorithm, we check the diagonal elements of the matrix (`matrix[i][i]` for each node `i`). If any of these elements are negative, it means that node `i` is part of a negative cycle.
  
2. **Propagating the Negative Cycle**:
   - If a negative cycle is detected, we then propagate this information. Any node that is reachable from or can reach a node involved in a negative cycle will have its distance set to `-1` to indicate that it is also affected by the negative cycle.

3. **Handling Nodes Affected by the Negative Cycle**:
   - By setting distances to `-1` for all nodes affected by a negative cycle, we ensure that the final result properly reflects the presence of these cycles. 

This modification allows the algorithm to detect and handle negative cycles in the graph.

---

Here's the modified version of your code with the handling for negative cycles:

```java
class Solution
{
    public void shortest_distance(int[][] matrix)
    {
        // Step 1: Initialize the matrix
        int n = matrix.length;

        // Step 2: Preprocess the matrix
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                // If there's no direct path between nodes i and j, set it to a large value (infinity)
                if (matrix[i][j] == -1)  
                    matrix[i][j] = (int)(1e9);

                // The distance from a node to itself should be 0
                if (i == j) 
                    matrix[i][j] = 0;       
            }
        }

        // Step 3: Floyd-Warshall Algorithm
        // Iterate over each possible intermediate node 'via'
        for (int via = 0; via < n; via++)
        {
            // For each pair of nodes (i, j), update the shortest path considering the intermediate node 'via'
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    // Update the distance between node i and node j
                    matrix[i][j] = Math.min
                    (
                        matrix[i][j], // The current shortest distance between i and j
                        matrix[i][via] + matrix[via][j] // The distance through the intermediate node 'via'
                    );
                }
            }
        }

        // Step 4: Check for negative cycles
        for (int i = 0; i < n; i++)
        {
            // If the distance from a node to itself becomes negative, it indicates a negative cycle
            if (matrix[i][i] < 0)
            {
                // Propagate the negative cycle indication
                for (int j = 0; j < n; j++)
                {
                    for (int k = 0; k < n; k++)
                    {
                        // If a node is reachable from any node involved in a negative cycle, set the distance to -1
                        if (matrix[i][j] != (int)(1e9) && matrix[i][k] != (int)(1e9))
                            matrix[j][k] = -1;
                    }
                }
            }
        }

        // Step 5: Post-process the matrix
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                // If the distance is still set to infinity, it means there's no path, so reset it to -1
                if (matrix[i][j] == (int)(1e9))  
                    matrix[i][j] = -1;
            }
        }
    }
}
```

## 35. City With the Smallest Number of Neighbors at a Threshold Distance
[City With the Smallest Number of Neighbors at a Threshold Distance | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/1)
###### Explanation:
1. **Initialization**: The `dist` array is initialized with `Integer.MAX_VALUE` to represent that there is no direct path between cities initially. The distance from each city to itself is set to 0.

2. **Populating Edges**: The `edges` array is used to populate direct distances between cities. Since the graph is undirected, the distance is set both ways.

3. **Floyd-Warshall Algorithm**: This algorithm is used to find the shortest paths between all pairs of cities. It considers each city as an intermediate point and updates the shortest path between every pair of cities.

4. **Finding the City with the Fewest Reachable Cities**: The code then counts how many cities each city can reach within the given `distanceThreshold`. It tracks the city with the minimum count. If there's a tie, the city with the larger number is chosen.

5. **Return Value**: Finally, the city with the fewest reachable cities is returned as the answer.

This code effectively solves the problem of finding the city that has the smallest number of neighbors within a certain distance.

```java
class Solution {
    int findCity(int n, int m, int[][] edges, int distanceThreshold) {
        // Initialize a 2D array to store the shortest distances between cities
        int[][] dist = new int[n][n];

        // Fill the distance array with a large value (infinity) to represent no direct path
        for (int[] d : dist) {
            Arrays.fill(d, Integer.MAX_VALUE);
        }

        // Populate the distance array with the edges given in the input
        // Each edge has a starting city (from), ending city (to), and a weight (wt)
        for (int[] g : edges) {
            int from = g[0];
            int to = g[1];
            int wt = g[2];
            dist[from][to] = wt;
            dist[to][from] = wt;  // Since it's an undirected graph, set distance both ways
        }

        // Set the distance from any city to itself as 0
        for (int i = 0; i < n; i++) {
            dist[i][i] = 0;
        }

        // Use the Floyd-Warshall algorithm to find the shortest path between all pairs of cities
        for (int via = 0; via < n; via++) {  // Consider each city as an intermediate point
            for (int i = 0; i < n; i++) {    // For each starting city
                for (int j = 0; j < n; j++) { // For each destination city
                    // If there's no direct path via the intermediate city, skip the update
                    if (dist[i][via] == Integer.MAX_VALUE || dist[via][j] == Integer.MAX_VALUE) continue;
                    
                    // Update the shortest distance if a shorter path is found via the intermediate city
                    dist[i][j] = Math.min(dist[i][j], dist[i][via] + dist[via][j]);
                }
            }
        }

        // Variables to keep track of the city with the fewest reachable cities within the distance threshold
        int cntCity = n;  // Start with the maximum possible number of cities
        int cityNo = -1;  // Variable to store the city with the minimum reachable cities

        // Loop through each city to determine how many cities can be reached within the distance threshold
        for (int city = 0; city < n; city++) {
            int cnt = 0;
            for (int adjCity = 0; adjCity < n; adjCity++) {
                // Count the number of cities reachable from the current city within the distance threshold
                if (dist[city][adjCity] <= distanceThreshold) {
                    cnt++;
                }
            }

            // Update the city with the fewest reachable cities
            // If there's a tie, choose the city with the larger number
            if (cnt <= cntCity) {
                cntCity = cnt;
                cityNo = city;
            }
        }

        // Return the city number with the fewest reachable cities within the distance threshold
        return cityNo;
    }
}
```


## 36. Minimum Spanning Tree - MST

```
Definition: A tree in which we have N nodes and N-1 edges and all nodes are reachable from each other. 
```

###### How to find the MST of the given graph

```
There are couple of algorithms which are used to find the MST of the given graph.

1. Prim's Algorithm
2. Kruskal's Algorightm - to understand the KA, need to know about "Disjoint Set".

```

## 37. Prim's Algorithm - MST
[Minimum Spanning Tree | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

Prim's Algorithm is a classic greedy algorithm used to find the Minimum Spanning Tree (MST) of a weighted, undirected graph. The MST is a subset of the graph's edges that connects all the vertices together without any cycles and with the minimum possible total edge weight.

###### Key Concepts:
- **Spanning Tree**: A subgraph that includes all the vertices of the original graph but only enough edges to form a tree, meaning no cycles and only one path between any two vertices.
- **Minimum Spanning Tree (MST)**: The spanning tree with the smallest possible total edge weight.

###### How Prim's Algorithm Works:
1. **Initialization**:
   - Start with an arbitrary node and add it to the MST.
   - Initialize a priority queue (or min-heap) to keep track of the edges with the smallest weight that connect to the growing MST.

2. **Growing the MST**:
   - At each step, select the edge with the minimum weight that connects a vertex in the MST to a vertex outside the MST.
   - Add this edge and the new vertex to the MST.
   - Update the priority queue with the new edges connecting the newly added vertex to the vertices not yet in the MST.

3. **Repeat**:
   - Repeat the process until all vertices are included in the MST.

4. **Termination**:
   - The algorithm stops when all vertices are part of the MST. At this point, you have the MST for the graph.

###### Example:
Let's say we have the following graph:

```
     2      3
A ------- B ----- C
 \       / \     /
  6    8    5  7
   \ /        X
    D ------- E
      9
```

1. **Starting Point**: Begin with vertex `A`. The adjacent edges are `A-B (2)` and `A-D (6)`. Choose the smallest edge, `A-B (2)`.

2. **Expand**: Now, `B` is in the MST. The available edges are `B-C (3)`, `B-D (8)`, and `A-D (6)`. The smallest is `B-C (3)`, so add `C` and `B-C (3)`.

3. **Continue**: Now, `A`, `B`, and `C` are in the MST. The available edges are `C-E (7)`, `B-D (8)`, and `A-D (6)`. The smallest edge is `A-D (6)`, so add `D` and `A-D (6)`.

4. **Final Step**: The only edge left is `C-E (7)`, which is the smallest edge available. Add `E` and `C-E (7)` to the MST.

The resulting MST connects all vertices with the minimum total weight of `2 + 3 + 6 + 7 = 18`.

###### Time Complexity:
Prim's Algorithm is efficient, with a time complexity of \(O((V + E) \log V)\) using a priority queue, where \(V\) is the number of vertices and \(E\) is the number of edges.

###### Explanation

```

1. **spanningTree Function**: This is the entry point of the solution. It calls the `solve` function, which implements Prim's algorithm to compute the Minimum Spanning Tree (MST).

2. **Priority Queue (Min-Heap)**: The priority queue is used to always pick the edge with the smallest weight, ensuring that the MST is constructed with the minimum possible total weight.

3. **Visited Array**: The `vis` array is used to keep track of the nodes that have been included in the MST to avoid cycles.

4. **Main Loop**: The loop continues until all nodes are processed. For each node, it adds the edge with the smallest weight to the MST if the node hasn't been visited.

5. **Adjacency List**: The adjacency list `adj` is used to store all the edges for each node, and it's traversed to find the next minimum edge to add to the MST.

6. **Pair Class**: The `Pair` class is used to store the node and the weight of the edge connecting to that node, which is useful when adding nodes to the priority queue.

This implementation of Prim's algorithm efficiently finds the MST with a time complexity of \(O((V + E) \log V)\), where \(V\) is the number of vertices and \(E\) is the number of edges.
```

```java
class Solution {
    // Function to calculate the weight of the minimum spanning tree (MST) using Prim's algorithm
    static int spanningTree(int V, int E, List<List<int[]>> adj) {
        // Call the helper function 'solve' to compute the MST and return its total weight
        return solve(V, E, adj);
    }
    
    // Helper function to implement Prim's algorithm and compute the MST weight
    private static int solve(int V, int E, List<List<int[]>> adj) {
        // Priority queue to pick the edge with the minimum weight
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.wt - b.wt);
        
        // Boolean array to track visited nodes
        boolean[] vis = new boolean[V];
        
        // Start the algorithm from node 0 with an initial weight of 0
        pq.offer(new Pair(0, 0));
        
        int sum = 0; // Variable to store the total weight of the MST
        
        // Process the nodes until the priority queue is empty
        while (!pq.isEmpty()) {
            // Get the node with the minimum weight edge from the priority queue
            Pair curr = pq.poll();
            int node = curr.node;
            int wt = curr.wt;
            
            // If the node is already visited, skip it to avoid cycles
            if (vis[node] == true) continue;
            
            // Mark the node as visited
            vis[node] = true;
            
            // Add the edge's weight to the total MST weight
            sum += wt;
            
            // Iterate through the adjacent nodes of the current node
            for (int[] nbs : adj.get(node)) {
                int adjNode = nbs[0];
                int adjWt = nbs[1];
                
                // If the adjacent node is not visited, add it to the priority queue
                if (vis[adjNode] == false) {
                    pq.offer(new Pair(adjNode, adjWt));
                }
            }
        }
        // Return the total weight of the MST
        return sum;
    }
    
    // Class to represent a pair of node and weight
    static class Pair {
        int node; // Node number
        int wt;   // Weight of the edge connecting to this node
        
        // Constructor to initialize the Pair
        Pair(int n, int w) {
            node = n; 
            wt = w;
        }
    }
}
```



## 38. Disjoint set | Union Find
```
## Union(u,v)
1. Find ultimate parent of u and v - pu, pv
2. Find rand of pu, pv.
3. Connect smaller rank to larger rank always - in case of equality connect anyone to anyone.
```

In graph theory and computer science, a "disjoint set" refers to a collection of non-overlapping sets, often used to manage and track elements partitioned into disjoint subsets. This concept is closely related to data structures that efficiently handle union and find operations on these subsets. 

Here’s a breakdown of the key concepts:

1. **Disjoint Set (Union-Find) Data Structure**: This is a data structure that supports two main operations efficiently:
   - **Find**: Determines which subset a particular element is in. This helps in identifying whether two elements are in the same subset.
   - **Union**: Merges two subsets into a single subset. 

2. **Applications**: Disjoint set data structures are commonly used in algorithms that deal with connectivity and partitioning, such as:
   - **Kruskal’s Algorithm**: This algorithm for finding the Minimum Spanning Tree (MST) of a graph uses disjoint sets to manage and merge the subsets of vertices to avoid cycles while constructing the MST.
   - **Network Connectivity**: Disjoint sets help in determining whether two nodes are in the same connected component of a graph.
   - **Equivalence Relations**: They are used in scenarios where equivalence classes need to be managed, such as in language parsing or database query optimization.

3. **Optimizations**:
   - **Path Compression**: This technique optimizes the find operation by making nodes in the path point directly to the root, speeding up future operations.
   - **Union by Rank/Size**: This optimization keeps the tree flat by attaching the smaller tree under the root of the larger tree, improving the efficiency of union operations.

**Example of Operations**:

- **Find**: If you want to determine the set containing element `x`, you follow parent pointers until you reach the root of the tree containing `x`.

- **Union**: To merge the sets containing elements `x` and `y`, you connect the roots of the trees containing `x` and `y`.

In summary, the disjoint set data structure is essential for efficiently managing and querying subsets in various applications involving partitioning and connectivity.

### Explanation:

1. **Disjoint Set Data Structure**: This code implements the Disjoint Set (or Union-Find) data structure with optimizations like union by rank and union by size, along with path compression. It efficiently handles dynamic connectivity queries and union operations.

2. **Initialization**:
   - The `DisjointSet` class constructor initializes three lists: `rank`, `parent`, and `size`.
   - Each element is initially its own parent, has a rank of 0, and a size of 1.

3. **Path Compression (`findUParent`)**:
   - This method finds the ultimate parent of a node while flattening the structure, making future queries faster.

4. **Union by Rank**:
   - This operation merges two sets based on their ranks (depth of trees), ensuring that the tree remains balanced and shallow.

5. **Union by Size**:
   - This operation merges two sets based on their sizes, attaching the smaller tree under the larger tree, again ensuring efficiency.

6. **Main Function**:
   - Various union operations are performed using `unionBySize`.
   - It checks if two elements (3 and 7) belong to the same set using the `findUParent` method.
   - After performing `unionByRank`, it checks again if the two elements are now in the same set.

This code effectively demonstrates the Disjoint Set operations, showcasing both union by size and union by rank optimizations along with path compression.

```java
class DisjointSet {
    // Lists to store rank, parent, and size of each element
    List<Integer> rank = new ArrayList<>();
    List<Integer> parent = new ArrayList<>();
    List<Integer> size = new ArrayList<>();

    // Constructor to initialize the disjoint set with n elements
    public DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            rank.add(0);        // Initially, all ranks are 0
            parent.add(i);      // Each element is initially its own parent
            size.add(1);        // The initial size of each set is 1
        }
    }

    // Method to find the ultimate parent of a node with path compression
    public int findUParent(int node) {
        // If the node is its own parent, return the node
        if (node == parent.get(node)) {
            return node;
        }
        // Recursively find the ultimate parent and apply path compression
        int ulp = findUParent(parent.get(node));
        parent.set(node, ulp); // Update the parent of the node to its ultimate parent
        return parent.get(node); // Return the ultimate parent
    }

    // Method to perform union by rank
    public void unionByRank(int u, int v) {
        // Find the ultimate parents of u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If they are already in the same set, return
        if (ulp_u == ulp_v) {
            return;
        }

        // Attach the smaller rank tree under the larger rank tree
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);
        } else {
            // If ranks are the same, make one the parent of the other and increase its rank
            parent.set(ulp_v, ulp_u);
            rank.set(ulp_u, rank.get(ulp_u) + 1);
        }
    }

    // Method to perform union by size
    public void unionBySize(int u, int v) {
        // Find the ultimate parents of u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If they are already in the same set, return
        if (ulp_u == ulp_v) {
            return;
        }

        // Attach the smaller size tree under the larger size tree and update the size
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Print "Hello World" to the console
        System.out.println("Hello World");

        // Initialize a DisjointSet with 7 elements
        DisjointSet ds = new DisjointSet(7);

        // Perform union by size on the sets containing elements 1 and 2
        ds.unionBySize(1, 2);

        // Perform union by size on the sets containing elements 2 and 3
        ds.unionBySize(2, 3);

        // Perform union by size on the sets containing elements 4 and 5
        ds.unionBySize(4, 5);

        // Perform union by size on the sets containing elements 6 and 7
        ds.unionBySize(6, 7);

        // Perform union by size on the sets containing elements 5 and 6
        ds.unionBySize(5, 6);

        // Check if the ultimate parents of 3 and 7 are the same (i.e., if they belong to the same set)
        if (ds.findUParent(3) == ds.findUParent(7)) {
            System.out.println("Same");
        } else {
            System.out.println("Not Same");
        }

        // Perform union by rank on the sets containing elements 3 and 7
        ds.unionByRank(3, 7);

        // Check again if the ultimate parents of 3 and 7 are the same
        if (ds.findUParent(3) == ds.findUParent(7)) {
            System.out.println("Same");
        } else {
            System.out.println("Not Same");
        }
    }
}
```


## 39. Kruskal's Algorithm

Kruskal's Algorithm is a popular algorithm used to find the Minimum Spanning Tree (MST) of a connected, undirected graph. The MST is a subset of the edges of a graph that connects all vertices together, without any cycles and with the minimum possible total edge weight.

###### Steps of Kruskal's Algorithm:

```
1. **Sort all edges**: First, sort all the edges of the graph in non-decreasing order of their weights.
  
2. **Initialize Disjoint Sets**: Each vertex is initially placed in its own disjoint set. This is typically done using a Disjoint Set data structure (also known as Union-Find).

3. **Pick the smallest edge**: Start with the smallest edge and check if adding it to the spanning tree will form a cycle. This is checked using the Disjoint Set data structure:
   - If the two vertices connected by the edge belong to different sets (i.e., they have different ultimate parents), adding this edge will not form a cycle, so you include this edge in the MST and merge the two sets.
   - If they belong to the same set (i.e., they have the same ultimate parent), adding this edge would form a cycle, so you skip it.

4. **Repeat**: Continue selecting the next smallest edge from the sorted list and repeat the process until you have included exactly \(V-1\) edges in the MST (where \(V\) is the number of vertices in the graph).

5. **Completion**: Once you have \(V-1\) edges in the MST, the algorithm terminates, and the selected edges represent the MST of the graph.
```
###### Characteristics:
- **Greedy Algorithm**: Kruskal's algorithm is a greedy algorithm, as it picks the smallest available edge at each step with the hope of building the MST.
  
- **Time Complexity**: The time complexity of Kruskal's algorithm is \(O(E \log E)\), where \(E\) is the number of edges. This complexity arises primarily from sorting the edges and performing union-find operations.

###### Applications:
- **Network Design**: Kruskal's algorithm is often used in network design problems where you want to connect multiple nodes (such as cities, computers, etc.) with the least total cost.
- **Approximation Algorithms**: It is also used as a basis for more complex approximation algorithms in cases where the exact solution may be hard to compute.

###### Example:
Consider a graph with the following edges and weights:

```
A --(1)-- B
A --(4)-- C
B --(2)-- C
C --(3)-- D
B --(5)-- D
```

Using Kruskal's Algorithm:

1. Sort the edges by weight: (A-B, 1), (B-C, 2), (C-D, 3), (A-C, 4), (B-D, 5)
2. Start with the smallest edge, A-B. Add it to the MST.
3. Next, consider B-C. Add it to the MST.
4. Next, consider C-D. Add it to the MST.
5. The next edge, A-C, forms a cycle with the already included edges, so skip it.
6. Similarly, B-D forms a cycle, so skip it.

The MST formed will include edges (A-B, 1), (B-C, 2), and (C-D, 3) with a total weight of 6.

---

[Minimum Spanning Tree | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/minimum-spanning-tree/1)

###### Explanation:

```
1. **Disjoint Set Class**:
   - **rank**: Helps in keeping the tree flat by attaching shorter trees under taller trees during union operations.
   - **parent**: Tracks the parent of each node. Initially, each node is its own parent.
   - **size**: Tracks the size of the sets, which is useful in the union by size method.

2. **findUParent**:
   - Recursively finds the ultimate parent (root) of a node. Implements path compression by making nodes point directly to the root, flattening the structure.

3. **unionByRank**:
   - Unites two sets based on the rank. The tree with a lower rank is attached under the tree with a higher rank to keep the tree as flat as possible.

4. **unionBySize**:
   - Unites two sets based on size. The smaller set is always attached under the larger set to maintain a balanced structure.

5. **Edge Class**:
   - Represents an edge with its weight and the two vertices it connects. Implements `Comparable` to allow sorting edges by their weight.

6. **spanningTree**:
   - Uses Kruskal’s algorithm to find the Minimum Spanning Tree (MST). It involves sorting edges by weight and then adding them to the MST if they don’t form a cycle.

7. **solve**:
   - Converts the adjacency list to an edge list, sorts the edges, and iteratively adds them to the MST using the union-find data structure.
```

```java
class Solution {

    // Disjoint Set data structure with union by rank and union by size functionalities
    static class DisjointSet {
        List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
        List<Integer> parent = new ArrayList<>(); // To store the parent of each node
        List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

        // Constructor to initialize the Disjoint Set for n elements
        public DisjointSet(int n) {
            // Initialize each element as its own parent, rank as 0, and size as 1
            for (int i = 0; i <= n; i++) {
                rank.add(0);     // Rank starts at 0 for all elements
                parent.add(i);   // Each element is its own parent initially
                size.add(1);     // Initial size of each set is 1
            }
        }

        // Function to find the ultimate parent (root) of a node with path compression
        public int findUParent(int node) {
            // If the node is its own parent, return the node
            if (node == parent.get(node)) {
                return node;
            }
            // Otherwise, recursively find the ultimate parent and apply path compression
            int ulp = findUParent(parent.get(node));
            parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
            return parent.get(node);
        }

        // Function to perform union of two sets by rank
        public void unionByRank(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by rank: attach the tree with lower rank under the tree with higher rank
            if (rank.get(ulp_u) < rank.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            } else {
                // If ranks are the same, attach one tree under the other and increase its rank
                parent.set(ulp_v, ulp_u);
                rank.set(ulp_u, rank.get(ulp_u) + 1);
            }
        }

        // Function to perform union of two sets by size
        public void unionBySize(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by size: attach the smaller set under the larger set
            if (size.get(ulp_u) < size.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
                size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
            } else {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
                size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
            }
        }
    }

    // Class to represent an edge with its weight and connected vertices
    static class Edge implements Comparable<Edge> {
        int weight;  // Weight of the edge
        int u;       // One vertex of the edge
        int v;       // The other vertex of the edge

        // Constructor to initialize an edge
        Edge(int weight, int u, int v) {
            this.weight = weight;
            this.u = u;
            this.v = v;
        }

        // Comparator function to sort edges based on their weight (ascending order)
        @Override
        public int compareTo(Edge other) {
            return this.weight - other.weight;
        }
    }

    // Function to find the weight of the Minimum Spanning Tree (MST) using Kruskal's algorithm
    static int spanningTree(int V, int E, List<List<int[]>> adj) {
        return solve(V, E, adj);
    }

    // Helper function to solve the MST problem
    private static int solve(int V, int E, List<List<int[]>> adj) {
        List<Edge> edges = new ArrayList<>();

        // Convert adjacency list representation to edge list representation
        for (int u = 0; u < V; u++) {
            for (int[] edge : adj.get(u)) {
                int v = edge[0];
                int wt = edge[1];
                edges.add(new Edge(wt, u, v));  // Add each edge to the edge list
            }
        }

        // Initialize Disjoint Set for all vertices
        DisjointSet ds = new DisjointSet(V);

        // Sort all edges based on their weight (increasing order)
        Collections.sort(edges);

        int mstWt = 0;  // To store the total weight of the MST

        // Iterate over all sorted edges
        for (Edge edge : edges) {
            int u = edge.u;
            int v = edge.v;
            int wt = edge.weight;

            // Check if the current edge forms a cycle in the MST
            if (ds.findUParent(u) != ds.findUParent(v)) {
                // If not, include this edge in the MST
                mstWt += wt;
                ds.unionBySize(u, v);  // Union the sets of the two vertices
            }
        }

        // Return the total weight of the MST
        return mstWt;
    }
}
```




## 40. [Number of Provinces](https://leetcode.com/problems/number-of-provinces/) | Using DisjointSet or Union Find

```java
class DisjointSet {
    // Lists to store rank, parent, and size of each element
    List<Integer> rank = new ArrayList<>();
    List<Integer> parent = new ArrayList<>();
    List<Integer> size = new ArrayList<>();

    // Constructor to initialize the disjoint set with n elements
    public DisjointSet(int n) {
        for (int i = 0; i <= n; i++) {
            rank.add(0);        // Initially, all ranks are 0
            parent.add(i);      // Each element is initially its own parent
            size.add(1);        // The initial size of each set is 1
        }
    }

    // Method to find the ultimate parent of a node with path compression
    public int findUParent(int node) {
        // If the node is its own parent, return the node
        if (node == parent.get(node)) {
            return node;
        }
        // Recursively find the ultimate parent and apply path compression
        int ulp = findUParent(parent.get(node));
        parent.set(node, ulp); // Update the parent of the node to its ultimate parent
        return parent.get(node); // Return the ultimate parent
    }

    // Method to perform union by rank
    public void unionByRank(int u, int v) {
        // Find the ultimate parents of u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If they are already in the same set, return
        if (ulp_u == ulp_v) {
            return;
        }

        // Attach the smaller rank tree under the larger rank tree
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);
        } else {
            // If ranks are the same, make one the parent of the other and increase its rank
            parent.set(ulp_v, ulp_u);
            rank.set(ulp_u, rank.get(ulp_u) + 1);
        }
    }

    // Method to perform union by size
    public void unionBySize(int u, int v) {
        // Find the ultimate parents of u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If they are already in the same set, return
        if (ulp_u == ulp_v) {
            return;
        }

        // Attach the smaller size tree under the larger size tree and update the size
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));
        } else {
            parent.set(ulp_v, ulp_u);
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));
        }
    }
}
class Solution {
    
    public int findCircleNum(int[][] isConnected) {
        return solve(isConnected);
    }

    private int solve(int[][] graph){
        int V =  graph.length;

        DisjointSet ds = new DisjointSet(V);
        for (int i=0; i<V; i++){
            for (int j=0; j<V; j++){
                if(graph[i][j] == 1){
                    ds.unionBySize(i,j);
                }
            }
        }
        int cnt = 0;
        for (int i=0; i<V; i++){
            if (ds.parent.get(i) == i){
                cnt++;
            }
        }
        return cnt;
    }
}
```

## 41. Connecting the graph
[Connecting the graph | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/connecting-the-graph/1)
###### Explanation:
```
- **Disjoint Set (Union-Find):**
  - **Initialization:** Each element is its own parent, with rank 0 and size 1.
  - **Find with Path Compression:** Ensures that each node points directly to its root, optimizing future queries.
  - **Union by Rank/Size:** Merges two sets by attaching the smaller tree under the larger one (either by rank or size), keeping the structure balanced.

- **Edge Representation:**
  - Represents a graph edge with a weight and two connected vertices. The `compareTo` method allows sorting edges by weight, which is useful in algorithms like Kruskal's.

- **Solve Method:**
  - Iterates through all edges, using union-find to detect and count redundant edges.
  - Counts connected components and determines the minimum number of edges needed to connect all components. If there are enough extra edges, the components can be connected; otherwise, it's impossible.
```

```java
class Solution {
    class DisjointSet {
        List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
        List<Integer> parent = new ArrayList<>(); // To store the parent of each node
        List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

        // Constructor to initialize the Disjoint Set for n elements
        public DisjointSet(int n) {
            // Initialize each element as its own parent, rank as 0, and size as 1
            for (int i = 0; i <= n; i++) {
                rank.add(0);     // Rank starts at 0 for all elements
                parent.add(i);   // Each element is its own parent initially
                size.add(1);     // Initial size of each set is 1
            }
        }

        // Function to find the ultimate parent (root) of a node with path compression
        public int findUParent(int node) {
            // If the node is its own parent, return the node
            if (node == parent.get(node)) {
                return node;
            }
            // Otherwise, recursively find the ultimate parent and apply path compression
            int ulp = findUParent(parent.get(node));
            parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
            return parent.get(node);
        }

        // Function to perform union of two sets by rank
        public void unionByRank(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by rank: attach the tree with lower rank under the tree with higher rank
            if (rank.get(ulp_u) < rank.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            } else {
                // If ranks are the same, attach one tree under the other and increase its rank
                parent.set(ulp_v, ulp_u);
                rank.set(ulp_u, rank.get(ulp_u) + 1);
            }
        }

        // Function to perform union of two sets by size
        public void unionBySize(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by size: attach the smaller set under the larger set
            if (size.get(ulp_u) < size.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
                size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
            } else {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
                size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
            }
        }
    }

    // Class to represent an edge with its weight and connected vertices
    class Edge implements Comparable<Edge> {
        int weight;  // Weight of the edge
        int u;       // One vertex of the edge
        int v;       // The other vertex of the edge

        // Constructor to initialize an edge
        Edge(int weight, int u, int v) {
            this.weight = weight;
            this.u = u;
            this.v = v;
        }

        // Comparator function to sort edges based on their weight (ascending order)
        @Override
        public int compareTo(Edge other) {
            return this.weight - other.weight;
        }
    }
    
    // Function to solve the problem using Disjoint Set
    public int Solve(int n, int[][] edge) {
        DisjointSet ds = new DisjointSet(n);  // Initialize the Disjoint Set for n nodes
        
        int extraEdges = 0;  // To count edges that connect already connected components
        
        int m = edge.length;  // Number of edges
        for (int i = 0; i < m; i++) {
            int u = edge[i][0];  // Start node of the edge
            int v = edge[i][1];  // End node of the edge
            // If u and v have the same ultimate parent, they are in the same set
            if (ds.findUParent(u) == ds.findUParent(v)) {
                extraEdges++;  // This edge is redundant (extra)
            } else {
                ds.unionBySize(u, v);  // Union the sets containing u and v
            }
        }
        
        int connectedComponents = 0;  // To count the number of connected components
        for (int i = 0; i < n; i++) {
            // If a node is its own parent, it's a root of a component
            if (ds.parent.get(i) == i) {
                connectedComponents++;
            }
        }
        
        int ans = connectedComponents - 1;  // Minimum edges needed to connect all components
        // If the number of extra edges is greater than or equal to the number of edges needed
        if (extraEdges >= ans) {
            return ans;  // Return the number of edges needed
        }
        return -1;  // Not enough extra edges to connect all components
    }
}
```

## 42. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

```java
class Solution {
    class DisjointSet {
        List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
        List<Integer> parent = new ArrayList<>(); // To store the parent of each node
        List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

        // Constructor to initialize the Disjoint Set for n elements
        public DisjointSet(int n) {
            // Initialize each element as its own parent, rank as 0, and size as 1
            for (int i = 0; i <= n; i++) {
                rank.add(0);     // Rank starts at 0 for all elements
                parent.add(i);   // Each element is its own parent initially
                size.add(1);     // Initial size of each set is 1
            }
        }

        // Function to find the ultimate parent (root) of a node with path compression
        public int findUParent(int node) {
            // If the node is its own parent, return the node
            if (node == parent.get(node)) {
                return node;
            }
            // Otherwise, recursively find the ultimate parent and apply path compression
            int ulp = findUParent(parent.get(node));
            parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
            return parent.get(node);
        }

        // Function to perform union of two sets by rank
        public void unionByRank(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by rank: attach the tree with lower rank under the tree with higher rank
            if (rank.get(ulp_u) < rank.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            } else {
                // If ranks are the same, attach one tree under the other and increase its rank
                parent.set(ulp_v, ulp_u);
                rank.set(ulp_u, rank.get(ulp_u) + 1);
            }
        }

        // Function to perform union of two sets by size
        public void unionBySize(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes share the same ultimate parent, they are already in the same set
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by size: attach the smaller set under the larger set
            if (size.get(ulp_u) < size.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
                size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
            } else {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
                size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
            }
        }
    }

    class Edge implements Comparable<Edge> {
        int weight;  // Weight of the edge
        int u;       // One vertex of the edge
        int v;       // The other vertex of the edge

        // Constructor to initialize an edge
        Edge(int weight, int u, int v) {
            this.weight = weight;
            this.u = u;
            this.v = v;
        }

        // Comparator function to sort edges based on their weight (ascending order)
        @Override
        public int compareTo(Edge other) {
            return this.weight - other.weight;
        }
    }
    public int[] findRedundantConnection(int[][] edge) {
        DisjointSet ds = new DisjointSet(edge.length);  // Initialize the Disjoint Set for n nodes
        
        int extraEdges = 0;  // To count edges that connect already connected components
        
        int m = edge.length;  // Number of edges
        int[] res = new int[2];
        for (int i = 0; i < m; i++) {
            int u = edge[i][0];  // Start node of the edge
            int v = edge[i][1];  // End node of the edge
            // If u and v have the same ultimate parent, they are in the same set
            if (ds.findUParent(u) == ds.findUParent(v)) {
                res[0]= u;  // This edge is redundant (extra)
                res[1]= v;  // This edge is redundant (extra)
            } else {
                ds.unionBySize(u, v);  // Union the sets containing u and v
            }
        }
        return res;
    }
}
```

## 43. Merging Deatils | Accounts Merge
###### Explanation:
1. **DisjointSet Class**:
   - **Fields**:
     - `rank`: Keeps track of the depth of each tree for union operations.
     - `parent`: Stores the parent of each node.
     - `size`: Keeps track of the size of each set for union operations.
   - **Methods**:
     - `findUParent(int node)`: Finds the root parent of `node` and performs path compression to flatten the tree.
     - `unionByRank(int u, int v)`: Unites two sets by attaching the smaller rank tree to the larger rank tree, or if equal, increase the rank of the new root.
     - `unionBySize(int u, int v)`: Unites two sets by attaching the smaller set to the larger set, and updates the size of the new root.

2. **accountsMerge Method**:
   - **Initialization**:
     - `DisjointSet ds`: Initializes the disjoint set for merging operations.
     - `mapMailNode`: Maps each email to its associated account index.
   - **Processing Accounts**:
     - For each email in an account, if it has already been encountered, perform a union operation to merge the current account with the account that has the same email.
   - **Merging Emails**:
     - Uses the `findUParent` method to determine the root parent and collects emails for each set.
   - **Preparing Final Output**:
     - Sorts the emails for each account, combines them with the account's name, and adds the result to the final list.

This code efficiently merges accounts that share the same email addresses, producing a list where each entry represents a unified account with sorted emails.

```java
import java.util.*;

// Class to handle the disjoint set (union-find) operations
class Solution {
    // Inner class to represent the Disjoint Set (Union-Find) data structure
    class DisjointSet {
        List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
        List<Integer> parent = new ArrayList<>(); // To store the parent of each node
        List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

        // Constructor to initialize the Disjoint Set for n elements
        public DisjointSet(int n) {
            // Initialize each element as its own parent, rank as 0, and size as 1
            for (int i = 0; i <= n; i++) {
                rank.add(0);     // Rank starts at 0 for all elements
                parent.add(i);   // Each element is its own parent initially
                size.add(1);     // Initial size of each set is 1
            }
        }

        // Function to find the ultimate parent (root) of a node with path compression
        public int findUParent(int node) {
            // If the node is its own parent, it is the root of its set
            if (node == parent.get(node)) {
                return node;
            }
            // Otherwise, recursively find the ultimate parent and apply path compression
            int ulp = findUParent(parent.get(node));
            parent.set(node, ulp);  // Path compression: make the ultimate parent the direct parent of the node
            return parent.get(node);
        }

        // Function to perform union of two sets by rank
        public void unionByRank(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes are already in the same set, no need to union
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by rank: attach the tree with lower rank under the tree with higher rank
            if (rank.get(ulp_u) < rank.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            } else {
                // If ranks are the same, attach one tree under the other and increase the rank of the new root
                parent.set(ulp_v, ulp_u);
                rank.set(ulp_u, rank.get(ulp_u) + 1);
            }
        }

        // Function to perform union of two sets by size
        public void unionBySize(int u, int v) {
            // Find the ultimate parents (roots) of the nodes u and v
            int ulp_u = findUParent(u);
            int ulp_v = findUParent(v);

            // If both nodes are already in the same set, no need to union
            if (ulp_u == ulp_v) {
                return;
            }

            // Union by size: attach the smaller set under the larger set
            if (size.get(ulp_u) < size.get(ulp_v)) {
                parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
                size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new root
            } else {
                parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
                size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new root
            }
        }
    }

    // Main function to merge accounts and return the result
    public List<List<String>> accountsMerge(List<List<String>> accounts) {
        int n = accounts.size();  // Number of accounts
        
        // Initialize Disjoint Set for each account
        DisjointSet ds = new DisjointSet(n);
        
        // Map to track which email belongs to which account
        Map<String, Integer> mapMailNode = new HashMap<>();
        int ii = 0; // Counter for the current account
        
        // Process each account
        for (List<String> acc : accounts) {
            for (int j = 1; j < acc.size(); j++) {
                String mail = acc.get(j);  // Email address
                
                // If the email is not in the map, add it
                if (!mapMailNode.containsKey(mail)) {
                    mapMailNode.put(mail, ii);
                } else {
                    // Union the current account with the account that contains the same email
                    ds.unionBySize(ii, mapMailNode.get(mail));
                }
            }
            ii++;
        }
        
        // List to hold the merged emails for each account
        List<String>[] mergedMail = new ArrayList[n];
        for (int i = 0; i < n; i++) {
            mergedMail[i] = new ArrayList<>();
        }
        
        // Add emails to the corresponding account's list based on their root parent
        for (Map.Entry<String, Integer> it : mapMailNode.entrySet()) {
            String mail = it.getKey();
            int node = ds.findUParent(it.getValue()); // Find the root parent of the account
            mergedMail[node].add(mail); // Add email to the merged list for that account
        }
        
        // List to store the final result
        List<List<String>> ans = new ArrayList<>();
        
        // Process each account's merged emails and prepare the final output
        for (int i = 0; i < n; i++) {
            if (mergedMail[i].size() == 0) {
                continue; // Skip if no emails are merged for this account
            }
            Collections.sort(mergedMail[i]); // Sort emails lexicographically
            List<String> temp = new ArrayList<>();
            temp.add(accounts.get(i).get(0)); // Add the account name
            temp.addAll(mergedMail[i]); // Add the sorted emails
            ans.add(temp); // Add the result to the final list
        }
        
        return ans; // Return the list of merged accounts
    }
}
```

## 44. Number of Islands II 
[Number Of Islands | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/number-of-islands/1)
###### Explanation:
1. **DisjointSet Class**:
   - `rank`, `parent`, and `size` lists keep track of the rank (or depth), parent, and size of each set respectively.
   - **Constructor** initializes each element to be its own parent, with rank 0 and size 1.
   - `findUParent(int node)` uses path compression to find the root of the set containing the node.
   - `unionByRank(int u, int v)` merges two sets based on the rank of their roots.
   - `unionBySize(int u, int v)` merges two sets based on the size of their roots.

2. **Solution Class**:
   - `numOfIslands(int r, int c, int[][] operators)` initializes grid dimensions and calls the `solve` method.
   - `solve(int[][] operators)` processes each operation, updating the grid and managing the number of islands using the `DisjointSet` class.
   - `isValid(int adjr, int adjc)` checks if a cell is within grid bounds.

This code maintains the number of islands dynamically as operations are applied to the grid, effectively using the union-find data structure to manage connectivity.


```java
// DisjointSet class to manage disjoint sets using union-find with path compression
class DisjointSet {
    List<Integer> rank = new ArrayList<>();   // To store the rank (or depth) of each set (used in union by rank)
    List<Integer> parent = new ArrayList<>(); // To store the parent of each node
    List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

    // Constructor to initialize the Disjoint Set for n elements
    public DisjointSet(int n) {
        // Initialize each element as its own parent, rank as 0, and size as 1
        for (int i = 0; i <= n; i++) {
            rank.add(0);     // Rank starts at 0 for all elements
            parent.add(i);   // Each element is its own parent initially
            size.add(1);     // Initial size of each set is 1
        }
    }

    // Function to find the ultimate parent (root) of a node with path compression
    public int findUParent(int node) {
        // If the node is its own parent, return the node (it is a root)
        if (node == parent.get(node)) {
            return node;
        }
        // Otherwise, recursively find the ultimate parent and apply path compression
        int ulp = findUParent(parent.get(node));
        parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
        return parent.get(node);
    }

    // Function to perform union of two sets by rank
    public void unionByRank(int u, int v) {
        // Find the ultimate parents (roots) of the nodes u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If both nodes share the same ultimate parent, they are already in the same set
        if (ulp_u == ulp_v) {
            return;
        }

        // Union by rank: attach the tree with lower rank under the tree with higher rank
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
        } else {
            // If ranks are the same, attach one tree under the other and increase its rank
            parent.set(ulp_v, ulp_u);
            rank.set(ulp_u, rank.get(ulp_u) + 1);
        }
    }

    // Function to perform union of two sets by size
    public void unionBySize(int u, int v) {
        // Find the ultimate parents (roots) of the nodes u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If both nodes share the same ultimate parent, they are already in the same set
        if (ulp_u == ulp_v) {
            return;
        }

        // Union by size: attach the smaller set under the larger set
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
        } else {
            parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
        }
    }
}

// Solution class to find the number of islands formed after each operation
class Solution {
    int rows; // Number of rows in the grid
    int cols; // Number of columns in the grid
    
    // Main function to return the number of islands after each operation
    public List<Integer> numOfIslands(int r, int c, int[][] operators) {
        rows = r;
        cols = c;
        return solve(operators);
    }
    
    // Function to process each operation and determine the number of islands
    private List<Integer> solve(int[][] operators) {
        DisjointSet ds = new DisjointSet(rows * cols); // Initialize Disjoint Set for a grid with rows*cols elements
        List<Integer> ans = new ArrayList<>(); // List to store the result after each operation
        int len = operators.length; // Number of operations
        boolean[][] vis = new boolean[rows][cols]; // Grid to keep track of visited cells
        int cnt = 0; // Counter for the number of islands
        
        // Process each operation
        for (int i = 0; i < len; i++) {
            int row = operators[i][0]; // Row index of the operation
            int col = operators[i][1]; // Column index of the operation
            
            // If the cell is already visited, no change in number of islands, just record the count
            if (vis[row][col]) {
                ans.add(cnt);
                continue;
            }
            
            // Mark the cell as visited and increment the island count
            vis[row][col] = true;
            cnt++;
            
            // Arrays to navigate through the four possible directions (up, right, down, left)
            int[] rowDiag = {-1, 0, 1, 0};
            int[] colDiag = {0, 1, 0, -1};
            
            // Check adjacent cells
            for (int ind = 0; ind < 4; ind++) {
                int adjRow = row + rowDiag[ind]; // Row index of adjacent cell
                int adjCol = col + colDiag[ind]; // Column index of adjacent cell
                
                // Check if the adjacent cell is within the grid bounds
                if (isValid(adjRow, adjCol)) {
                    if (vis[adjRow][adjCol]) { // If the adjacent cell is visited
                        int nodeNo = row * cols + col; // Current cell's unique number
                        int adjNodeNo = adjRow * cols + adjCol; // Adjacent cell's unique number
                        
                        // If current cell and adjacent cell are in different sets, perform union
                        if (ds.findUParent(nodeNo) != ds.findUParent(adjNodeNo)) {
                            cnt--; // Decrease island count as two separate islands are now merged
                            ds.unionBySize(nodeNo, adjNodeNo); // Merge the sets
                        }
                    }
                }
            }
            
            // Record the number of islands after this operation
            ans.add(cnt);
        }
        return ans; // Return the final list of island counts after each operation
    }

    // Helper function to check if the given cell is within the grid bounds
    private boolean isValid(int adjr, int adjc) {
        return adjr >= 0 && adjr < rows && adjc >= 0 && adjc < cols;
    }
}
```


## 45. Making a Large Island | [Maximum Connected group | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/maximum-connected-group/1)

###### Explanation:

1. **DisjointSet Class**:
   - **Fields**:
     - `rank`: Stores the rank (or depth) of each set. This helps with union by rank, but in this implementation, only union by size is used.
     - `parent`: Stores the parent of each node. Each node points to itself initially.
     - `size`: Stores the size of the set that each node belongs to.
   - **Constructor**:
     - Initializes each element to be its own parent with a size of 1 and rank of 0.
   - **Methods**:
     - `findUParent(int node)`: Finds the root parent of a node with path compression.
     - `unionBySize(int u, int v)`: Merges the sets containing `u` and `v` by size, ensuring that the smaller set is attached to the larger set.

2. **Solution Class**:
   - **Fields**:
     - `rows` and `cols`: Dimensions of the grid.
   - **Main Function (`MaxConnection`)**:
     - Initializes the Disjoint Set for all cells in the grid.
     - **Step 1**: Iterate through each cell. For cells with value `1`, union them with adjacent cells that are also `1`.
     - **Step 2**: For each cell with value `0`, calculate the potential size of the connected component if that cell were changed to `1`. Use a set to keep track of unique connected components that the cell would connect with.
     - Also consider the case when no `0` is changed, to ensure the maximum size is captured.
   - **Helper Function (`isValid`)**:
     - Checks if a given cell `(r, c)` is within the grid bounds.

This code efficiently finds the size of the largest connected component that can be obtained by changing at most one `0` to `1` in a grid.

```java
import java.util.*;

// Class to handle the disjoint set (union-find) operations with union by size
class DisjointSet {
    List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
    List<Integer> parent = new ArrayList<>(); // To store the parent of each node
    List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

    // Constructor to initialize the Disjoint Set for n elements
    public DisjointSet(int n) {
        // Initialize each element as its own parent, rank as 0, and size as 1
        for (int i = 0; i < n; i++) {  // Corrected to 'i < n'
            rank.add(0);     // Rank starts at 0 for all elements
            parent.add(i);   // Each element is its own parent initially
            size.add(1);     // Initial size of each set is 1
        }
    }

    // Function to find the ultimate parent (root) of a node with path compression
    public int findUParent(int node) {
        // If the node is its own parent, return the node
        if (node == parent.get(node)) {
            return node;
        }
        // Otherwise, recursively find the ultimate parent and apply path compression
        int ulp = findUParent(parent.get(node));
        parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
        return ulp;
    }

    // Function to perform union of two sets by size
    public void unionBySize(int u, int v) {
        // Find the ultimate parents (roots) of the nodes u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If both nodes share the same ultimate parent, they are already in the same set
        if (ulp_u == ulp_v) {
            return;
        }

        // Union by size: attach the smaller set under the larger set
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
        } else {
            parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
        }
    }
}

// Class to find the maximum connected group in a grid
class Solution {
    int rows;  // Number of rows in the grid
    int cols;  // Number of columns in the grid

    // Main function to find the maximum size of the connected component
    public int MaxConnection(int grid[][]) {
        rows = grid.length;
        cols = grid[0].length;  // Number of columns in the grid

        DisjointSet ds = new DisjointSet(rows * cols);  // Initialize Disjoint Set for all cells in the grid

        // Directions for moving up, down, left, right
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, -1, 0, 1};

        // Step 1: Union adjacent cells that are `1`
        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (grid[row][col] == 1) {
                    int nodeNo = row * cols + col; // Convert (row, col) to a unique node index
                    for (int ind = 0; ind < 4; ind++) {
                        int newR = row + dr[ind];
                        int newC = col + dc[ind];
                        // Check if the new position is within bounds and is part of a connected group
                        if (isValid(newR, newC) && grid[newR][newC] == 1) {
                            int adjNode = newR * cols + newC; // Convert adjacent cell to a unique node index
                            ds.unionBySize(nodeNo, adjNode); // Union the current cell with the adjacent cell
                        }
                    }
                }
            }
        }

        // Step 2: Calculate the maximum size of a connected component if one `0` is changed to `1`
        int maxSize = 0;

        for (int row = 0; row < rows; row++) {
            for (int col = 0; col < cols; col++) {
                if (grid[row][col] == 0) {
                    Set<Integer> set = new HashSet<>();
                    // Check all 4 possible directions from the current `0` cell
                    for (int ind = 0; ind < 4; ind++) {
                        int newR = row + dr[ind];
                        int newC = col + dc[ind];
                        // Check if the new position is within bounds and is part of a connected group
                        if (isValid(newR, newC) && grid[newR][newC] == 1) {
                            set.add(ds.findUParent(newR * cols + newC)); // Find the root of the connected component
                        }
                    }
                    int sizeTotal = 1;  // We are considering the `0` we're changing to `1`
                    // Calculate the total size of the connected component that includes the new `1`
                    for (int it : set) {
                        sizeTotal += ds.size.get(it);
                    }
                    maxSize = Math.max(maxSize, sizeTotal); // Update the maximum size
                }
            }
        }

        // Also consider the case when no `0` is changed, the largest connected component
        for (int cellNo = 0; cellNo < rows * cols; cellNo++) {
            maxSize = Math.max(maxSize, ds.size.get(ds.findUParent(cellNo))); // Update the maximum size
        }

        return maxSize; // Return the size of the largest connected group
    }

    // Function to check if the cell (r, c) is within grid bounds
    private boolean isValid(int r, int c) {
        return r >= 0 && r < rows && c >= 0 && c < cols;
    }
}
```

## 46. [Maximum Stone Removal | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/maximum-stone-removal-1662179442/1)\

###### Explanation:
1. **DisjointSet Class**:
   - **Fields**:
     - `rank`: Used to store the rank of each set (though rank is not used in the `maxRemove` method, it is initialized).
     - `parent`: Keeps track of the parent of each node.
     - `size`: Used to store the size of each set (used in union by size).
   - **Constructor**:
     - Initializes each element to be its own parent with a size of 1. The `rank` is initialized to 0, although it is not utilized in the `maxRemove` method.
   - **Methods**:
     - `findUParent(int node)`: Finds the root parent of a node with path compression to optimize future queries.
     - `unionByRank(int u, int v)`: Unions two sets based on their rank. This method is not used in the `maxRemove` function.
     - `unionBySize(int u, int v)`: Unions two sets based on their size, attaching the smaller set under the larger set to keep the tree shallow.

2. **Solution Class**:
   - **Function (`maxRemove`)**:
     - **Purpose**: Calculates the maximum number of stones that can be removed such that the remaining stones are still connected in a grid-like structure.
     - **Steps**:
       - **Find Maximum Indices**: Determine the maximum row and column indices to size the disjoint set correctly.
       - **Initialize Disjoint Set**: Create a Disjoint Set with enough space to handle row and column indices.
       - **Process Stones**: For each stone, union the row index with the column index (offset by `maxRow + 1` to avoid overlap in the disjoint set).
       - **Count Unique Connected Components**: Count how many unique components are formed (nodes that are their own parent).
       - **Calculate Result**: The maximum number of stones that can be removed is the total number of stones minus the number of unique connected components.

This code efficiently finds how many stones can be removed by using the disjoint set data structure to track connectivity between rows and columns in the grid.


```java
import java.util.*;

// Class to handle the disjoint set (union-find) operations with union by rank and union by size
class DisjointSet {
    List<Integer> rank = new ArrayList<>();   // To store the rank of each set (used in union by rank)
    List<Integer> parent = new ArrayList<>(); // To store the parent of each node
    List<Integer> size = new ArrayList<>();   // To store the size of each set (used in union by size)

    // Constructor to initialize the Disjoint Set for n elements
    public DisjointSet(int n) {
        // Initialize each element as its own parent, rank as 0, and size as 1
        for (int i = 0; i <= n; i++) {
            rank.add(0);     // Rank starts at 0 for all elements
            parent.add(i);   // Each element is its own parent initially
            size.add(1);     // Initial size of each set is 1
        }
    }

    // Function to find the ultimate parent (root) of a node with path compression
    public int findUParent(int node) {
        // If the node is its own parent, return the node
        if (node == parent.get(node)) {
            return node;
        }
        // Otherwise, recursively find the ultimate parent and apply path compression
        int ulp = findUParent(parent.get(node));
        parent.set(node, ulp);  // Path compression: set the parent to the ultimate parent
        return parent.get(node);
    }

    // Function to perform union of two sets by rank
    public void unionByRank(int u, int v) {
        // Find the ultimate parents (roots) of the nodes u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If both nodes share the same ultimate parent, they are already in the same set
        if (ulp_u == ulp_v) {
            return;
        }

        // Union by rank: attach the tree with lower rank under the tree with higher rank
        if (rank.get(ulp_u) < rank.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
        } else if (rank.get(ulp_v) < rank.get(ulp_u)) {
            parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
        } else {
            // If ranks are the same, attach one tree under the other and increase its rank
            parent.set(ulp_v, ulp_u);
            rank.set(ulp_u, rank.get(ulp_u) + 1);
        }
    }

    // Function to perform union of two sets by size
    public void unionBySize(int u, int v) {
        // Find the ultimate parents (roots) of the nodes u and v
        int ulp_u = findUParent(u);
        int ulp_v = findUParent(v);

        // If both nodes share the same ultimate parent, they are already in the same set
        if (ulp_u == ulp_v) {
            return;
        }

        // Union by size: attach the smaller set under the larger set
        if (size.get(ulp_u) < size.get(ulp_v)) {
            parent.set(ulp_u, ulp_v);  // Attach u's tree under v's tree
            size.set(ulp_v, size.get(ulp_v) + size.get(ulp_u));  // Update the size of the new tree
        } else {
            parent.set(ulp_v, ulp_u);  // Attach v's tree under u's tree
            size.set(ulp_u, size.get(ulp_u) + size.get(ulp_v));  // Update the size of the new tree
        }
    }
}

// Class to solve the problem of finding the maximum number of stones that can be removed
class Solution {
    // Function to find the maximum number of stones that can be removed
    int maxRemove(int[][] stones, int n) {
        // Variables to keep track of the maximum row and column indices
        int maxRow = 0;
        int maxCol = 0;

        // Find the maximum row and column indices in the stones array
        for (int i = 0; i < n; i++) {
            maxRow = Math.max(maxRow, stones[i][0]);
            maxCol = Math.max(maxCol, stones[i][1]);
        }

        // Initialize Disjoint Set for all cells in the grid (with additional space for columns)
        DisjointSet ds = new DisjointSet(maxRow + maxCol + 1);

        // Map to keep track of nodes that are part of stones
        HashMap<Integer, Integer> stoneNodes = new HashMap<>();

        // Process each stone and union its row and column
        for (int i = 0; i < n; i++) {
            int nodeRow = stones[i][0];
            int nodeCol = stones[i][1] + maxRow + 1; // Offset column indices to avoid overlap

            // Union the row and column of the current stone
            ds.unionBySize(nodeRow, nodeCol);

            // Record the presence of these nodes in the stoneNodes map
            stoneNodes.put(nodeRow, 1);
            stoneNodes.put(nodeCol, 1);
        }

        // Count the number of unique connected components
        int cnt = 0;
        for (Map.Entry<Integer, Integer> it : stoneNodes.entrySet()) {
            if (ds.findUParent(it.getKey()) == it.getKey()) {
                cnt++; // Count each unique root
            }
        }

        // Return the number of stones that can be removed, which is total stones minus the number of unique connected components
        return n - cnt;
    }
}
```


