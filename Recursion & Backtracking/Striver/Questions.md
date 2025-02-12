#### [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

```java
// TC: O(2^n) nearly, i.e., exponential 
class Solution {
    public int fib(int n) {
        return f(n);
    }

    private int f(int n){
        if (n <= 1)
            return n;

        int last = f(n-1);
        int sLast = f(n-2);

        return last + sLast;
    }
}
```

Optimization Solution is DP



#### Subsequences
A contagious or non-contagious sequences, which follows the order. 
###### Example
```java
arr = {3, 1, 2}

3
1
2
3, 1
1, 2
3, 2
3, 1, 2
```

**Note**: Order has to be maintain!
##### Print all Subsequences - Popular Algorithm (Power Set)

Blueprint | Template for take and not take 
```java
// TC: O(2^n X n)
// SC: O(N)
public class Subsequence  
{  
    public void f(int ind,int []arr, List<Integer> ds)  
    {        // base case  
        if (ind >= arr.length)  
        {   printSubSeq(ds);  
            return;  
        }  
        ds.add(arr[ind]);  
        f(ind + 1, arr, ds); // take  
        ds.remove(ds.size()-1);  
        f(ind + 1, arr, ds); // not take  
    }  
  
    private void printSubSeq(List<Integer> ds) {  
        if (ds.isEmpty())  
        {            System.out.println("{}");  
            return;  
        }        for (int it : ds)  
        {            System.out.print(it +" ");  
        }        System.out.println();  
    }
}
```


#### All SubSequences where sum = k
```java
import java.util.*;

/**
 * This class finds all possible subsequences in an array whose elements sum up to a target value.
 * It uses backtracking to generate all possible combinations.
 */
public class SubsequenceWithSum {
    // Input array to process
    private static int[] numbers;
    // Target sum we're looking for
    private static int targetSum;
    // List to store all valid subsequences
    private static List<List<Integer>> validSubsequences;

    public static void main(String[] args) {
        // Initialize input values
        numbers = new int[]{1, 2, 1};
        targetSum = 2;
        validSubsequences = new ArrayList<>();

        // Start the recursive backtracking process
        findSubsequences(0, new ArrayList<>(), 0);

        // Print all found subsequences
        for (List<Integer> subsequence : validSubsequences) {
            System.out.println(subsequence);
        }
    }

    /**
     * Recursive method to find all subsequences with the target sum.
     * Uses backtracking to explore all possible combinations.
     *
     * @param index Current index in the array being processed
     * @param currentSubsequence List containing current subsequence being built
     * @param currentSum Running sum of elements in current subsequence
     */
    private static void findSubsequences(int index, List<Integer> currentSubsequence, int currentSum) {
        // Base case: reached end of array
        if (index == numbers.length) {
            // If current sum equals target sum, add subsequence to results
            if (currentSum == targetSum) {
                validSubsequences.add(new ArrayList<>(currentSubsequence));
            }
            return;
        }

        // Include current element in subsequence
        currentSubsequence.add(numbers[index]);
        currentSum += numbers[index];
        findSubsequences(index + 1, currentSubsequence, currentSum);

        // Backtrack: exclude current element from subsequence
        currentSubsequence.remove(currentSubsequence.size() - 1);
        currentSum -= numbers[index];
        findSubsequences(index + 1, currentSubsequence, currentSum);
    }
}
```
#### Merge Sort Algorithm
Link: https://www.geeksforgeeks.org/problems/merge-sort/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article
```java
class Solution {

    void mergeSort(int arr[], int l, int r) {
        // code here
        f(arr,l,r);
    }
    
    private void f(int[] arr, int low, int high){
        if ( low >= high){
            return;
        }
        
        int mid = low+(high-low)/2;
        f(arr,low,mid);
        f(arr,mid+1,high);
        merge(arr,low,mid,high);
    }
    
    private void merge(int[] arr,int low, int mid, int high){
        int left=low;
        int right = mid+1;
        List<Integer> temp = new ArrayList<>();
        
        while (left <= mid && right <= high){
            if (arr[left] <= arr[right]){
                temp.add(arr[left]);
                left++;
            }
            else{
                temp.add(arr[right]);
                right++;
            }
        }
        
        while (left <= mid){
            temp.add(arr[left++]);
        }
        while (right <= high){
            temp.add(arr[right++]);
        }
        
        for (int i=low; i <= high; i++){
            arr[i] = temp.get(i-low);
        }
    }
}
```
The reason for using `arr[i] = temp.get(i - low);` is to correctly map the indices of the `temp` list back to the original array `arr`. Here's a detailed explanation:

###### Understanding the Problem
1. **Temp List (`temp`)**
   - `temp` is used to temporarily store the merged elements during the merge step.
   - Its indices start from `0` because it's a newly created list.

2. **Original Array (`arr`)**
   - `arr` spans from `low` to `high` for the current segment being merged.
   - This means the indices of `temp` need to be adjusted to match the corresponding indices in `arr`.

###### Why Use `(i - low)`?
The loop in the merge method iterates over the range `[low, high]` for `arr`. If you directly use `i` as the index for `temp`, it will attempt to access an out-of-bounds index because `temp` starts at `0`.

- The mapping between `arr` and `temp` works as follows:
  - `arr[low]` corresponds to `temp[0]`.
  - `arr[low + 1]` corresponds to `temp[1]`.
  - `arr[high]` corresponds to `temp[high - low]`.

Hence, to correctly map the indices, you subtract `low` from `i` when accessing `temp`:
```java
arr[i] = temp.get(i - low);
```

###### Without the Adjustment:
If you used `temp.get(i)` directly, you'd get an `IndexOutOfBoundsException` because `temp` is smaller (it only contains the merged elements for the range `[low, high]`).

###### Visualization
Let's say:
- `arr = [8, 3, 6, 2]`
- Current range to merge: `low = 1`, `high = 3`
- Merged result: `temp = [2, 3, 6]`

When copying `temp` back to `arr`:
- `arr[1]` gets `temp[0]` → `i - low = 1 - 1 = 0`.
- `arr[2]` gets `temp[1]` → `i - low = 2 - 1 = 1`.
- `arr[3]` gets `temp[2]` → `i - low = 3 - 1 = 2`.

Using `arr[i] = temp.get(i - low);` ensures the correct element is placed at the correct index in `arr`.


#### Quick Sort
https://leetcode.com/problems/sort-an-array/description/
Steps: 
1. Find the pivot, and place it on their correct position
2. Find the first element which is greater than pivot (----->)
3. Find the first element which is smaller than the pivot (<-------)
4. If i and j have not crossed each other, then do a swap

###### TC & SC
```
TC: O(n log n)
SC: O(1)
```

Code:
```java
class Solution {
    public int[] sortArray(int[] nums) {
        qs(nums,0,nums.length-1);
        return nums;
    }

    private void qs(int[] arr, int low, int high){
        if (low < high){
            int partitionInd = f(arr,low,high);
            qs(arr,low,partitionInd-1);
            qs(arr,partitionInd+1,high);
        }
    }


    private int f(int[] arr, int low, int high){
        int pivotIndex = low + (int) (Math.random() * (high - low + 1));
        swap(arr, low, pivotIndex); // Swap random pivot to the start
        int pivot = arr[low];
        int i = low;
        int j = high;

        while (i < j){
            // find the first largest
            while (arr[i] <= pivot && i <= high-1){
                i++;
            }
            while (arr[j] > pivot && j >= low+1){
                j--;
            }

            // check If i haven't crossed j
            if ( i < j)
                swap(arr,i,j);

        }
        swap(arr,low,j);
        return j;
    }

    private void swap(int[] arr, int i, int j){
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```
