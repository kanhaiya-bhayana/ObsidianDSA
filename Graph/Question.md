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