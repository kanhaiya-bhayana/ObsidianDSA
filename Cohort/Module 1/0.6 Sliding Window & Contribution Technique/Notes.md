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



```markdown
# Revision Notes: Subarrays and Optimization Techniques

## Introduction to Subarrays
A subarray is a contiguous portion of an array. For any given array, subarrays can be formed using different combinations of starting and ending indices but must maintain order.

### Basic Concepts
- **Number of Subarrays**: For an array of length `n`, the number of subarrays possible is `n * (n + 1) / 2`. This arises from choosing a start and end point for subarrays【8:19†source】.

## Sum of All Subarray Sums
To find the sum of sums of all possible subarrays:
- **Brute Force Method**: Use a triple nested loop to iterate, calculate, and sum each subarray's sum.
  - Time Complexity: $O(n^3)$
  - Space Complexity: $O(1)$
- **Optimization via Prefix Sum**: Instead of recalculating from scratch each time, use prefix sums to derive subarray sums efficiently.
  - Time Complexity: $O(n^2)$
  - Space Complexity: $O(n)$, due to storage of the prefix sums【8:0†source】【8:14†source】.

## Techniques Discussed
### Prefix Sum
- **Definition**: A prefix sum array allows quick computation of the sum of elements within any subarray by storing cumulative totals.
- **Usage in Optimization**: Convert subarray sum computation from $O(n^2)$ time to $O(n)$, using a single additional array【8:5†source】【8:8†source】.

### Contribution Technique
- **Definition**: Evaluate the contribution of each element to all subarrays it is part of. Specifically, for each element at index `i`, calculate how many subarrays it's part of and weight its contribution accordingly【8:13†source】【8:16†source】.
- **Formula**: For an element at index `i`, the total contribution to all subarrays is given by multiplying its value `A[i]` with `(i + 1) * (n - i)`, where `n` is the total number of elements. This finds how many times a particular element contributes to the sum of all subarrays【8:13†source】【8:16†source】.

## Optimization Challenges and Examples
### Range Sum Queries
- Tackling fixed-length subarrays allows applications of sliding window techniques alongside prefix sums for optimized solutions【8:15†source】.

### Equilibrium Index
- Identify an index such that the sum of elements on its left is equal to the sum on its right. Can effectively use prefix sums to achieve O(n) complexity【8:9†source】【8:15†source】.

## Key Takeaways
- **Practice**: The session stressed the importance of continuous practice to solidify these concepts. Techniques like prefix sum should become reflexive through repetition【8:5†source】【8:18†source】.
- **Algorithm Choice**: Competence in choosing the appropriate method (brute force vs. optimized approaches) based on context (e.g., input size, desired complexity) is crucial【8:17†source】.

## Conclusion
Combining knowledge of subarray properties and efficient computation techniques like prefix sums and the contribution method can significantly enhance your ability to solve related problems in a computationally efficient manner. Always consider the context and constraints to select the appropriate strategy.

---

Revisit this guide and practice coding these algorithms to build fluency in managing subarray challenges.
```
