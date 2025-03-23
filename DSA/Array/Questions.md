## Print all Permutations

```java
class Solution {

    List<List<Integer>> res;
    int[] arr;
    public void nextPermutation(int[] nums) {
        res = new ArrayList<>();
        arr = nums;

        backtrack(new ArrayList<>(), new boolean[arr.length]);

        for (List<Integer> it : res){
            System.out.println(it);
        }

        if (res.contains())
    }


    private void backtrack (List<Integer> ds, boolean[] vis){
        if (ds.size() == arr.length){
            res.add(new ArrayList<>(ds));
            return;
        }
        else{
            for (int i=0; i<arr.length; i++){
                if (vis[i]){
                    continue;
                }
                ds.add(arr[i]);
                vis[i] = true;
                backtrack(ds,vis);
                ds.remove(ds.size()-1);
                vis[i] = false;
            }
        }
    }
}
```