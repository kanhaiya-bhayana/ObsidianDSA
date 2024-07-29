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

