
### Q. Calculate the GCD of an entire array


##### code
```java
int ans = arr[0];
for (int i=1; i<n; i++){
	ans = gcd(ans,arr[i]);
}
```

### Q. Count Pair Sum #PairSum

**Problem Description**  

You are given an array **A** of **N** integers and an integer **B**. Count the number of pairs `(i,j)` such that `A[i] + A[j] = B` and `i ≠ j`.  
  
Since the answer can be very large, return the remainder after dividing the count with `109+7`.  
  
**Note** - The pair `(i,j)` is same as the pair `(j,i)` and we need to count it only once.

  
  
**Problem Constraints**  

1 <= N <= 105  
1 <= A[i] <= 109  
1 <= B <= 109

  
  
**Input Format**  

First argument A is an array of integers and second argument B is an integer.

  
  
**Output Format**  

Return an integer.

  
  
**Example Input**  

Input 1:

A = [3, 5, 1, 2]
B = 8

Input 2:

A = [1, 2, 1, 2]
B = 3

  
  
**Example Output**  

Output 1:

1

Output 2:

4

  
  
**Example Explanation**  

Example 1:

The only pair is (1, 2) which gives sum 8

Example 2:

The pair which gives sum as 3 are (1, 2), (1, 4), (2, 3) and (3, 4).

```java
public class Solution {

    public int solve(ArrayList<Integer> A, int B) {
        HashMap<Integer, Integer> frequencyMap = new HashMap<>();
        long pairCount = 0;
        long MOD = 1000000007;

        for (int num : A) {
            int complement = B - num;
            if (frequencyMap.containsKey(complement)) {
                pairCount += (long)frequencyMap.get(complement);
            }
            frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
        }

        return (int)(pairCount % MOD);
    }
}
```

> TC: O(N)
> SC: O(N)


#### #LongestSubString
### Given a string s, find the length of the longest substring without repeating characters. 

#### Brute Force Approach

>For all substrings, check if no characters are repeating and compare length

##### Code
```java
 public int lengthOfLongestSubstring(String s){
	 int maxLength = 0;
	 
	 for (int i=0; i<s.length(); i++){
		 HashSet<Charater> set = new HashSet<>()
		 int currLength = 0;
		 
		 for (int j=i; j<s.length(); j++){
			char c = s.charAt(j);
			
			if (set.contains(c)) 
				break;
			
			set.add(c);
			currLength++;
		 }
		 
		 maxLength = Math.max(maxLength, currLength);
	 }
	 
	 return maxLength;
 }
```

>TC: O(n^2)
>SC: O(n)


#### Better Approach | Sliding Window #SlidingWindow

```java
public int lengthOfSubstring(String s){
	int maxLength = 0;
	int left = 0;
	HashSet<Character> set = new HashSet<>();
	
	for (int right = 0; right < s.length; right++){
		char c = s.charAt(right);
		
		while (set.contains(c)){
			set.remove(s.charAt(left));
			left++;
		}
		
		set.add(c);
		maxLength = Math.max(maxLength, right - left + 1);
	}
}
```

> TC: O(n)
> SC: O(n)


### Q. Given an array of N elements, find the first non-repeating element.

#### Brute Force approach
> For every element of the array, iterate and find the frequency.
> The first element with frequency = 1 is the answer.


>TC: O(n^2)
>SC: O(1)

#### Optimize | HashMap

> Create a frequency map.
> Iterate over the array and find the first element with frequency 1.

> TC: O(n)
> SC: O(n)


### Q. Given an array of N elements, check if there exits a subarray with a sum equal to 0.

>Hint ==> PrefixSum + HashMap / HashSet

#### Brute Force approach

```java
for (i = 0; i <n-1; i++){
	for (j = i to n-1){
		sum = 0;
		
		for (k = i to j){
			sum += arr[k];
		}
		
		if (sum == 0) return true;
	}
}

return false;
```

> TC: O(n^3)
> SC: O(1)

#### Better - Carry Forward technique

```java
for (i=0 to n-1){
	sum = 0;
	for (j = i to n-1){
		sum ++ a[j];
		if (sum == 0){
			return true;
		}
	}
}

return false;
```

>TC: O(n^2)
>SC: O(1)
#### Optimized approach

> Take a variable sum, and check if the sum exists in the HashSet or sum is 0, if yes then return true, else add the sum into the HashSet.

```java
public int solve(int[] arr){
	
	if (arr == null || arr.length == 0){
		return 0;
	}
	
	for (int a : arr){
		if (a == 0) return 1;
	}
	
	Set<Long> set = new HashSet<>(); 
	long sum = 0;
	
	for (int a : arr){
		sum += a;
		
		if (set.contains(sum) || sum == 0){
			return 1;
		}	
		
		set.add(sum);
	}
	
	return 0;
}
```

> TC: O(n)
> SC: O(n)
### Q. Given an array of N elements, check if there exits a subarray with a sum equal to K.

```java
public int solve(int[] arr, int k){
	
	if (arr == null || arr.length == 0){
		return 0;
	}
	
	Set<Long> set = new HashSet<>();
	
	long sum = 0;
	
	for (int a : arr){
		sum += a;
		
		if (set.contains(sum - k)){
			return 1;
		}
		
		set.add(sum);
	}
	
	return 0;
}
```