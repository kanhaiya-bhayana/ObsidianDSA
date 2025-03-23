## 1. [1480. Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/)

```java
class Solution {
    public int[] runningSum(int[] nums) {
        int[] preSum = new int[nums.length];
        preSum[0] = nums[0];

        for (int i=1;i<nums.length; i++){
            preSum[i] = nums[i] + preSum[i-1];
        }
        
        return preSum;
    }
}
```

## 2. [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

```java
class NumArray {
    int[] preSum;
    public NumArray(int[] nums) {
        for (int i=1; i<nums.length; i++){
            nums[i] = nums[i-1] + nums[i];
        }
        preSum = nums;
    }
    
    public int sumRange(int left, int right) {
        return (left == 0) ? preSum[right] : preSum[right] - preSum[left-1];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(left,right);
 */
```

## 3. [724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;

        int[] pre = new int[n];
        int[] post = new int[n];

        pre[0] = nums[0];
        post[n-1] = nums[n-1];

        for (int i=1; i<n; i++){
            pre[i] = pre[i-1] + nums[i];
        }

        for (int i=n-2; i>=0; i--){
            post[i] = post[i+1] + nums[i];
        }

        for (int i=0; i<n; i++){
            if (pre[i] == post[i]){
                return i;
            }
        }

        return -1;
    }
}
```


## [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

```java
    public int longestSubarray(int[] arr, int k) {
        // code here
		Map<Integer,Integer> map = new HashMap<>();
    
        int n = arr.length;
        int curSum = 0;
		int maxL = 0;
		
		for (int i=0; i<arr.length; i++){
			
			curSum += arr[i];
			
			if (curSum == k){
			    maxL = i+1;
			}
			
			int diff = curSum - k;
			
			if (map.containsKey(diff)){
				maxL = Math.max (maxL, i - map.get(diff));
			}
			
			if (!map.containsKey(curSum)){
				map.put(curSum, i);
			}
		}
		
		return maxL;
    }
```