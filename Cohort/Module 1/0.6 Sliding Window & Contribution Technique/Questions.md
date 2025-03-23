### Print all the subarrays
```
TC -> O(n^3)
SC -> O(1)
```
### Sum of all the subarrays

##### Better Approach
###### Carry forward approach

```java
public int sumSubArray(int[] arr){
	int total = 0;
	
	for (int i=0; i<arr.length; i++){
		int sum = 0;
		for (int j=i; j<arr.length; j++){
			sum += arr[j];
			total += sum;
		}
	}
	
	return total;
}
/*
	-------------------------
	TC -> O(n^2)
	SC -> O(1)
	-------------------------
*/
```

##### Optimized Approach
###### Contribution Technique

```java
// arr[0]*x + arr[1]*y + arr[2]*z + ............... + arr[n]*m

// No. of subarrays the ith element will be a part of 
// (i+1) * (n-i)


public int findSumOfAllSubArraySums(int[] arr){
	int total = 0;
	
	for (int i=0; i<arr.length; i++){
		int contrib = arr[i] * ((i+1) * (n-i));
		int total += contrib;
	}
	
	return total;
}
/*
	---------------------------
	TC -> O(n)
	SC -> O(1)
	---------------------------
*/

```


### Total number of subarrays of length K

###### if n is given and k is given, how many possible total no. of subarrays of length k?

```
(N-k+1)
```


###### Given an array of size N, print start and end index of subarrays of length K?

```java
for (int i=0; i<n-k+1; i++){
	sout("start: " + i + "end: " + i+k-1);
}
```

### Given an array , find the maximum subarray sum for all subarrays of length k.

#### Brute force

> For every subarray of length K iterate and find sum.
> Get max of all such sums.
###### Carry forward approach

#### Optimal approach

###### Sliding Window

```java
inf maxSubArraySumLenK(int[] arr, int k){
	int max = Integer.MIN_VALUE;
	int sum = 0;
	
	// first window calculation
	for (int i=0; i<k; i++){
		sum += arr[k];
	}
	
	max = Math.max(sum, max);
	
	int i = 1;
	int j = i + k - 1;
	
	while (j < arr.length){
		sum -= arr[i-1];
		sum += arr[j];
		max = Math.max(sum, max);
		i++;
		j++;
	}
	
	return max;
}

/*
	---------------------------
	TC -> O(n)
	SC -> O(1)
	---------------------------
*/

```

```java
public class Solution {

    public int solve(ArrayList<Integer> A, int B) {
        int cnt = 0;
        int i = 0;
        int j = 0;
        int sum = 0;
        int n = A.size();
        while (j < n){
            sum += A.get(j);
            while (sum >= B && i <=j){
                sum -= A.get(i);
                i++;
            }
            if (sum < B){
                cnt += (j-i+1);
            }
            j++;
        }
        return cnt;
    }
}
```