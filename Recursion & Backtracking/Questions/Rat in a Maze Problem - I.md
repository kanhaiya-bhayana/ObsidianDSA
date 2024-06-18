
## Rat in a Maze Problem - I https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1

```java
//{ Driver Code Starts
// Initial Template for Java

import java.util.*;
class Rat {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            int n = sc.nextInt();
            int[][] a = new int[n][n];
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++) a[i][j] = sc.nextInt();

            Solution obj = new Solution();
            ArrayList<String> res = obj.findPath(a, n);
            Collections.sort(res);
            if (res.size() > 0) {
                for (int i = 0; i < res.size(); i++)
                    System.out.print(res.get(i) + " ");
                System.out.println();
            } else {
                System.out.println(-1);
            }
        }
    }
}

// } Driver Code Ends


// User function Template for Java

// m is the given matrix and n is the order of matrix

class Solution {
    public static int [][]visited;
public static ArrayList<String> res;
    public static ArrayList<String> findPath(int[][] m, int n) {
        // Your code here
        visited = new int[n][n];
        res = new ArrayList<>();
        if (m[0][0] == 0)
        {
            return res;
        }
        String s = "";
        findPaths(m,0,0,s,n);
        return res;
    }
    
    static void findPaths(int [][]input, int row, int col, String s, int n)
    {
        // base case
        if (row == n - 1 && col == n -1)
        {
            res.add(s);
            return;
        }
        else{
            visited[row][col] = 1;
        }
        
        if (row + 1 < n && visited[row+1][col] == 0 && input[row+1][col] == 1) // check the row, check haven't visited, check there is a path in input
        {
            findPaths(input, row + 1, col, s + "D", n); // DOWN
        }
        
        if (col -1 >= 0 && visited[row][col-1] == 0 && input[row][col-1] == 1)
        {
            findPaths(input, row, col -1, s+"L", n); // LEFT
        }
        
        if (col +1 < n && visited[row][col+1] == 0 && input[row][col+1] == 1)
        {
            findPaths(input, row, col+1, s+"R",n); // RIGHT
        }
        
        if (row-1 >= 0 && visited[row-1][col] == 0 && input[row-1][col] == 1)
        {
            findPaths(input,row-1,col,s+"U",n); // UP
        }
        visited[row][col] = 0;
        
    }
}
```



### Optimized code

```java
//{ Driver Code Starts
// Initial Template for Java

import java.util.*;
class Rat {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int t = sc.nextInt();

        while (t-- > 0) {
            int n = sc.nextInt();
            int[][] a = new int[n][n];
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++) a[i][j] = sc.nextInt();

            Solution obj = new Solution();
            ArrayList<String> res = obj.findPath(a, n);
            Collections.sort(res);
            if (res.size() > 0) {
                for (int i = 0; i < res.size(); i++)
                    System.out.print(res.get(i) + " ");
                System.out.println();
            } else {
                System.out.println(-1);
            }
        }
    }
}

// } Driver Code Ends


// User function Template for Java

// m is the given matrix and n is the order of matrix

class Solution {
    public static int [][]visited;
public static ArrayList<String> res;
public static int di[];
public static int dj[];
    public static ArrayList<String> findPath(int[][] m, int n) {
        // Your code here
        visited = new int[n][n];
        res = new ArrayList<>();
        if (m[0][0] == 0)
        {
            return res;
        }
        di = new int[]{1, 0, 0,-1};
        dj = new int[]{0, -1, 1, 0};
        findPaths(m,0,0,"",n);
        return res;
    }
    
    static void findPaths(int [][]input, int row, int col, String move, int n)
    {
        // base case
        if (row == n - 1 && col == n -1)
        {
            res.add(move);
            return;
        }
        
        String dir = "DLRU";
        for (int i = 0; i < 4; i++)
        {
            int nexti = row + di[i];
            int nextj = col + dj[i];
            if (nexti >= 0 && nextj >= 0 && nexti < n && nextj < n && visited[nexti][nextj] == 0 && input[nexti][nextj] == 1)
            {
                visited[row][col] = 1;
                findPaths(input,nexti,nextj, move+dir.charAt(i),n);
                visited[row][col] = 0;
            }
        }
        
    }
}
```