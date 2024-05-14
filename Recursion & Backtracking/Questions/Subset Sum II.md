https://leetcode.com/problems/subsets-ii/description/

```java
class Solution {
    Set<List<Integer>> res;
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        res = new HashSet<>();
        Arrays.sort(nums);
        dfs(0,new ArrayList<>(),nums);
        return new ArrayList<>(res);
    }

    private void dfs(int ind, List<Integer> sub, int []arr)
    {
        // base case
        if (ind == arr.length)
        {
            res.add(new ArrayList<>(sub));
            return;
        }

        sub.add(arr[ind]);
        dfs(ind+1,sub,arr);
        sub.remove(sub.size()-1);
        dfs(ind+1,sub,arr);
    }
}
```