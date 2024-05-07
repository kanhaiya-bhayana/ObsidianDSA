
#### Time Complexity
	2^t * k
	bcz t target will reduce every time and we have a recursion call for each t
#### Space complexity
		k *x
		k - avg length
		x - combinations


https://leetcode.com/problems/combination-sum/description/

```java
class Solution {
    List<List<Integer>> res;
    int []arr;
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        res = new ArrayList<>();
        arr = candidates;
        combinationSumUtil(0, new ArrayList<>(),target);
        return res;
    }

    private void combinationSumUtil(int ind, List<Integer> ds, int target)
    {
        // base case
        if (ind == arr.length)
        {
            if (target == 0)
            {
                res.add(new ArrayList<>(ds));
            }
            return;
        }

        // pick
        if (arr[ind] <= target)
        {
            ds.add(arr[ind]);
            combinationSumUtil(ind, ds, target-arr[ind]);
            ds.remove(ds.size()-1);
        }
        // not pick 
        combinationSumUtil(ind+1, ds,target);
    }
}
```