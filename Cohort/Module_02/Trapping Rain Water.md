
### Given the height of the N buildings. Find the rain water trapped between the buildings.

### [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) #LeetCode

```java
public class Solution {

    // DO NOT MODIFY THE LIST. IT IS READ ONLY

    public int trap(final List<Integer> A) {
        int[] arr = new int[A.size()];
        
        for (int i=0; i<arr.length; i++){
            arr[i] = A.get(i);
        }

        int n = arr.length;
        int []water = new int[n];

        int []maxL = new int [n];
        int []maxR = new int [n];

        maxL[0] = arr[0];
        maxR[n-1] = arr[n-1];

        for (int i = 1; i < n; i++){
            maxL[i] = Math.max(maxL[i-1],arr[i]);
        }

        for (int i = n - 2; i >= 0; i--){
            maxR[i] = Math.max(maxR[i+1], arr[i]);
        }

        for (int i = 0; i < n; i++){
            water[i] = Math.min(maxL[i],maxR[i]) - arr[i];
        }

        int res = 0;

        for (int it : water){
            res += it;
        }

        return res;
    }

}
```

### #TrappingRainWater
#### Approach 1 - Brute Force

```
For every building calculate max height of a building in left and right.

Use the max height to calculate the height of water on top of the building.
```

##### Code

```java
func(int[] arr){
	int n = arr.length;
	int ans = 0;
	
	for (int i=1; i<(n-1); i++){
		int maxL = arr[i];
		
		for (int j=(i-1); j>=0; j--){ // for max left
			maxL = Math.max(arr[j], maxL);
		}
		int maxR = arr[i];
		for (int j=(i+1); j<n; j++){ // for max right
			maxR = Math.max(arr[j], maxR);
		}
		
		int water = Math.min(maxL, maxR) - A[i];
		
		ans += water;
	}
	
	return ans;
}

// TC: O(N * (N+N)) => O(N^2)
// SC: O(1)
```

#### Approach 2 - Carry forward | prefixSum & sufixSum

##### Code

```java
func(int[] arr){
	int n = arr.length;
	int[] prefixSum = new int[n];
	int[] sufixsum = new int[n];
	
	// prefixSum for left max
	prefixSum[0] = arr[0];
	for (int i=1; i<n; i++){
		prefixSum[i] = Math.max(prefixSum[i-1], arr[i]);
	}
	
	// sufixSum for right max
	sufixSum[n-1] = arr[n-1];
	for (int i=n-2; i>=0; i--){
		sufixSum = Math.max(sufixSum[i+1], arr[i]);
	}
	
	int ans = 0;
	for (int i=1; i<n-1; i++){
		ans += Math.min(prefixSum[i],sufixSum[i]) - arr[i];
	}
	
	return ans;
}

// TC: O(N)
// SC: O(N)
```