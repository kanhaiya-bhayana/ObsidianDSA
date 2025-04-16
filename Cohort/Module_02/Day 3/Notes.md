### Print Boundary Elements
### Given a NxN matrix. Print the boundary elements in clockwise direction

#### Pseudo Code 
```java
func (int[][] mat){
	int n = mat.length;
	
	int i = 0;
	int j = 0;
	
	// first row
	for (int k = 0; K<n-1; k++){
		print(mat[i][j]);
		j++;
	}
	
	// last col
	for (int k = 0; K<n-1; k++){
		print(mat[i][j]);
		i++;
	}
	
	// last row
	for (int k = 0; K<n-1; k++){
		print(mat[i][j]);
		j--;
	}
	
	// first col
	for (int k = 0; K<n-1; k++){
		print(mat[i][j]);
		i--;
	}
}
```

### #SpiralMatrix
### Q. Print a matrix of size NxN in a spiral way clockwise.

##### Code

```java
func (int[][] mat){
	int n = mat.length;
	
	int i = 0;
	int j = 0;
	
	while (n > 1){
		// first row
		for (int k = 0; K<n-1; k++){
			print(mat[i][j]);
			j++;
		}
		
		// last col
		for (int k = 0; K<n-1; k++){
			print(mat[i][j]);
			i++;
		}
		
		// last row
		for (int k = 0; K<n-1; k++){
			print(mat[i][j]);
			j--;
		}
		
		// first col
		for (int k = 0; K<n-1; k++){
			print(mat[i][j]);
			i--;
		}
		i++;
		j++;
		N=N-2;
	}
	
	if (n == 1){
		print(mat[i][j]);
	}
}

// TC: O(N^2)
// SC: O(1)
```


### #NextPermutation
### Given a number in the form of an array. Every index represents a digit

> For n number there will be n! permutations of length n 
##### Constraint
```
SC = O(1)
```

#### Brute Force

**Steps**
```
1. Generate all sorted permutations - recursion
2. Linear Search
3. return next ind permutation
```

```
TC: O(N! x N)
```

#### Better
#### 3 Step Process
#### Next Permutation Steps (Crisp Recap)

1. **Find Pivot**  
    → Traverse from end, find first `i` where `nums[i] < nums[i+1]`.
    
2. **Edge Case**  
    → If no pivot (`pivot = -1`), reverse entire array → return.
    
3. **Swap Successor**  
    → Find rightmost value `> nums[pivot]` → swap with pivot.
    
4. **Reverse Suffix**  
    → Reverse subarray after pivot index to get smallest possible sequence.
    

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

---
##### Code
```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;

        int pivot = -1;

        // Find pivot
        for (int i=n-2; i>=0; i--){
            if (nums[i] < nums[i+1]){
                pivot = i;
                break;
            }
        }   

        if (pivot == -1){
            reverse(nums,0, n-1);
            return;
        }

        // right most fo the pivot
        for (int i=n-1; i>=0; i--){
            if (nums[i] > nums[pivot]){
                swap(nums,i,pivot);
                break;
            }
        }

        int i = pivot+1;
        int j = n-1;
        reverse(nums,i,j);
        return;
    }

    private void reverse(int[] arr, int i, int j){
        while (i <=j){
            swap(arr,i++,j--);
        }
    }

    private void swap(int[] arr,int i, int j){
        int t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }

}
```