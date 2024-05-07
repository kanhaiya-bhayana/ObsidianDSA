
https://leetcode.com/problems/subsets-ii/description/

```java
class Solution {
    Set<List<Integer>> res;
    int []arr;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        res = new HashSet<>();
        arr = nums;
        Arrays.sort(arr);
        dfs(0, new ArrayList<>());
        return new ArrayList<>(res);
    }

    private void dfs(int ind, List<Integer> sub)
    {
        // base case
        if (ind >= arr.length)
        {
            res.add(new ArrayList<>(sub));
            return;
        }

        sub.add(arr[ind]);
        dfs(ind + 1, sub);  // take

        sub.remove(sub.size()-1);
        dfs(ind + 1, sub); // not take
    }
}
```

