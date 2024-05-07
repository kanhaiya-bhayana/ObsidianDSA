
```java
private void dfs(int ind, int sum)
    {
        // base case
        if (ind >= arr.size())
        {
            res.add(sum);
            return;
        }
        
        sum += arr.get(ind);
        dfs(ind + 1, sum); // take
        
        sum -= arr.get(ind);
        dfs(ind + 1, sum); // not take
    }
```

