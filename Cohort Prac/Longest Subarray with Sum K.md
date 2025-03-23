
[Longest Subarray with Sum K | Practice | GeeksforGeeks](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1?utm_source=youtube&utm_medium=collab_striver_ytdescription&utm_campaign=longest-sub-array-with-sum-k)
#### Better
```java

int len = 0;
for (int i=0; i<n; i++){
	int sum = 0;
	for (int j=i; j<n; j++){
		sum += arr[j];
		if (sum == k){
			len = Math.max(len, j-i+1);
		}
	}
}

return len;

// TC --> O(n^2)
```

#### Optimal

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