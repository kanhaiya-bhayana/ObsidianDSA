### Given an integer array A, find the maximum subarray sum out of all subarrays.

### LeetCode [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

#### Approach 1 - Brute Force

> Check sum of all subarrays, and pick the max.

> For length n, the total subarrays will be: n*(n+1)/2 --> n^2

###### Preferable
Always tell Time and Space Complexity first, then write code in the interview.

```
TC: O(n^3)  -> we have total n^2 subarrays as per the formulae and we have calculate n time i.e., n^3
SC: O(1)
```

###### Code

```java
func(int[] arr){
	int n = arr.length;
	int ans = arr[0];
	
	for (int i=0; i<n; i++){
		for (int j=i; j<n; j++){
			int sum = 0;
			for (int k=i; k<=j; k++){
				sum += arr[k];
			}
			ans = Math.max(ans,sum);
		}
	}
	return ans;
}
```

---
#### Approach 2 - Prefix Sum

Use prefix sum for calculating subarray sum of each array,

```
TC: O(n + n^2) ==> O(n^2), where n is for creating the prefix arr
SC: O(n)
```

---
#### Approach 3 - Carry Forward

Use carry forward for calculating subarray sum of each query.

```
TC: O(n^2) 
SC: O(1)
```
###### Code

```java
func(int[] arr){
	int n = arr.length;
	int ans = arr[0];
	
	for (int i=0; i<n; i++){
		int sum = 0;
		for (int j=i; j<n; j++){
			sum += arr[j];
			ans = Math.max(ans,sum);
		}
	}
	return ans;
}
```

---
#### Approach 4
##### Case 1: If all elements are positive, 
	then sum of whole array and return.
##### Case 2: If all elements are negative, 
	then max element
##### Case 3: If both, contains positive and negative
	take the positive chunk

#KadanesAlgorithm
#### Kadane's Algorithm

```
TC: O(n) 
SC: O(1)
```

##### Code

```java
func(int[] arr){
	int max = arr[0];
	int curr = 0;
	
	for (int i=0; i<n; i++){
		curr += arr[i];
		max - Math.max(curr, max);
		
		if (curr < 0){
			curr = 0;
		}
	}
	
	return max;
}
```

###### If you want to keep the track of L and R

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
        int curr = 0;
        int n = nums.length;

        int start = 0;
        int l = 0;
        int r = 0;

        for (int i=0; i<n; i++){
            curr += nums[i];
            
            if (curr > max){
                max = curr;
                l = start;
                r = i;
            }

            if (curr < 0){
                curr = 0;
                start = i + 1;
            }
        }

        System.out.println(l + " , " + r);
        return max;
    }
}
```