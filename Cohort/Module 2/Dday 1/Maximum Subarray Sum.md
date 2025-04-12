### Given an integer array A, find the maximum subarray sum out of all subarrays.

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
TC: (n + n^2) ==> O(n^2), where n is for creating the prefix arr
SC: O(n)
```

---
#### Approach 3 - Carry Forward

Use carry forward for calculating subarray sum of each query.

```
TC: (n^2) 
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
