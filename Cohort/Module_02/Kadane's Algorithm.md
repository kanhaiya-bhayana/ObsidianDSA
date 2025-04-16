#### Kadane's Algorithm

```
TC: O(n) 
SC: O(1)
```

#### #KadanesAlgorithm
##### Code

```java
func(int[] arr){
	int max = arr[0];
	int curr = 0;
	
	for (int i=0; i<n; i++){
		curr += arr[i];
		max = Math.max(curr, max);
		
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

---
#### #KadanesAlgorithm
## A Problem based on Kadane's Algorithm, not Directly
**Problem Description**  

You are given a binary string **A**(i.e., with characters **0** and **1**) consisting of characters **A1**, **A2**, ..., **AN**. In a single operation, you can choose two indices, **L** and **R,** such that **1** ≤ **L** ≤ **R** ≤ **N** and flip the characters **AL**, **AL+1**, ..., **AR**. By flipping, we mean changing character **0** to **1** and vice-versa.

Your aim is to perform **ATMOST** one operation such that in the final string number of **1s** is maximized.

If you don't want to perform the operation, return an **empty** array. Else, return an array consisting of two elements denoting **L** and **R**. If there are multiple solutions, return the **lexicographically smallest** pair of **L** and **R**.

**NOTE:** Pair **(a, b)** is lexicographically smaller than pair **(c, d)** if **a** < **c** or, if **a** == **c** and **b** < **d**.

  
  
**Problem Constraints**  

1 <= size of string <= 100000

  
  
**Input Format**  

First and only argument is a string A.

  
  
**Output Format**  

Return an array of integers denoting the answer.

  
  
**Example Input**  

Input 1:

A = "010"

Input 2:

A = "111"

  
  
**Example Output**  

Output 1:

[1, 1]

Output 2:

[]

  
  
**Example Explanation**  

Explanation 1:

A = "010"

Pair of [L, R] | Final string
_______________|_____________
[1 1]          | "110"
[1 2]          | "100"
[1 3]          | "101"
[2 2]          | "000"
[2 3]          | "001"

We see that two pairs [1, 1] and [1, 3] give same number of 1s in final string. So, we return [1, 1].

Explanation 2:

No operation can give us more than three 1s in final string. So, we return empty array [].

  
  

Expected Output

Provide sample input and click run to see the correct output for the provided input. Use this to improve your problem understanding and test edge cases


```java
public class Solution {

    public ArrayList<Integer> flip(String A) {

        int n = A.length();
        int[] diffArray = new int[n];
        boolean allOnes = true;

        // 1 --> -1
        // 0 -->  1

        for (int i=0; i<n; i++){
            if (A.charAt(i) == '1'){
                diffArray[i] = -1;
            }
            else{
                diffArray[i] = 1;
                allOnes = false;
            }
        }

        if (allOnes) return new ArrayList<>();

        int start = 0;
        int end = 0;
        int tStart = 0;
        int max = 0;
        int currSum = 0;
  
        for (int i=0; i<diffArray.length; i++){
            currSum += diffArray[i];
            if (currSum > max){
                max = currSum;
                start = tStart;
                end = i;
            }
            if (currSum < 0){
                currSum = 0;
                tStart = i+1;
            }
        }

        ArrayList<Integer> res = new ArrayList<>();
        res.add(start+1);
        res.add(end+1);

        return res;
    }

}
```