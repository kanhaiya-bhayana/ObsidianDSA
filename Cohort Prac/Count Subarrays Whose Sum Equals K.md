[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer,Integer> map = new HashMap<>();
        map.put(0, 1);

        int res =0;
        int sum = 0;
        
        for (int n : nums){
            sum += n;

            int diff = sum - k;

            if (map.containsKey(diff)){
                res += map.get(diff);
            }

            map.put(sum, map.getOrDefault(sum,0)+1);
        }

        return res;
    }
}
```