
#### Convert Adjacency Metrix to List

```java
private List<List<Integer>> constructAdjList(int[][] arr)
    {
        int V = arr.length;
        List<List<Integer>> adj = new ArrayList<>();

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
```

 ```java
 private List<List<Integer>> getAdjacencyList(int[][] prerequisites, int V) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int[] edge : prerequisites) {
            adj.get(edge[0]).add(edge[1]);
        }        
        return adj;
    }
```

```java
static List<List<Integer>> getAdjacencyList(ArrayList<ArrayList<Integer>> arr,int V, int m)
    {
        
        List<List<Integer>> adj = new ArrayList<>();
        for (int i=0; i<V; i++)
        {
            adj.add(new ArrayList<>());
        }
        
        for (ArrayList<Integer> ele: arr)
        {
            adj.get(ele.get(1)).add(ele.get(0));
        }
        
        return adj;
    }
```

---
```java
private List<List<Integer>> getAdjMat(int[][] graph)
    {
        int V = graph.length;
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int i = 0; i < V; i++) {
            for (int j : graph[i]) {
                adj.get(i).add(j);
            }
        }
        return adj;
    }
```
---
###### for reverse the edges
```java
private List<List<Integer>> getAdjMat(int[][] graph)
    {
        int V = graph.length;
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        
        for (int i = 0; i < V; i++) {
            for (int j : graph[i]) {
                adj.get(j).add(i);
            }
        }
        return adj;
    }
```