


TC : n! x n
SC : O(n) + O(n) // for ds, and map array


https://leetcode.com/problems/permutations/description/

```java
class Solution {
    List<List<Integer>> res;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();

        dfs(nums, new ArrayList<>(), new boolean[nums.length]); // nums, ds, boolean array as a map
        return res;
    }


    private void dfs(int []arr, List<Integer> ds, boolean[] freq)
    {
        // base case
        if (ds.size() == arr.length)
        {
            res.add(new ArrayList<>(ds));
            return;
        }

        for (int i = 0; i < arr.length; i++)
        {
            if (!freq[i]) // if not taken 
            {
                freq[i] = true;
                ds.add(arr[i]);
                dfs(arr,ds,freq);
                ds.remove(ds.size()-1);
                freq[i] = false;
            }
        }
    }
}
```




TC - n! x n
SC - O(1)


### Without extra space

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    List<List<Integer>> res;

    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<>();
        dfs(0, nums); 
        return res;
    }

    private void dfs(int ind, int[] arr) {
        // base case
        if (ind == arr.length) {
            List<Integer> list = new ArrayList<>();
            for (int num : arr) {
                list.add(num);
            }
            res.add(new ArrayList<>(list));
            return;
        }

        for (int i = ind; i < arr.length; i++) {
            swap(i, ind, arr);
            dfs(ind + 1, arr);
            swap(i, ind, arr);
        }
    }

    private void swap(int i, int j, int[] arr) {
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```


