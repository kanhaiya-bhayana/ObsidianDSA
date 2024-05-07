
https://www.geeksforgeeks.org/problems/subset-sums2234/1

```java
// User function Template for Java//User function Template for Java
class Solution {
    ArrayList<Integer> res;
    ArrayList<Integer> arr;
    ArrayList<Integer> subsetSums(ArrayList<Integer> input, int n) {
        // code here
        res = new ArrayList<>();
        arr = input;
        dfs(0, 0);
        return res;
    }
    
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
        dfs(ind + 1, sum);
    }
}
```