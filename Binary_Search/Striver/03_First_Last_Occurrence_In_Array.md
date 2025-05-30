## Find the first and last occurrence of x

> First --> lower bound
> Last --> upper bound 

! but make sure we have to handle the edge cases, like if lb == n or arr[lb] != x then return -1, i.e., ele does not exists.

```java

public int[] findFirstAndLast(int[] arr, int n, int x) {
    int lb = lowerBound(arr, n, x);
    
    // Check if x is not found
    if (lb == n || arr[lb] != x) {
        return new int[]{-1, -1};
    }
    return new int[]{lb, upperBound(arr, n, x) - 1};
}

public int lowerBound(int[] arr, int n, int x){
	int low = 0;
	int high = n-1;
	int res = n; // hypothetical answer
	
	while (low <= high){
		int mid = low + (high - low) /2;
		if (arr[mid] > x){
			res = mid;
			high = mid - 1; // look for more small index on left
		}
		else{
			low = mid + 1; // look to the right
		}
	}
	return res;
}

public int lowerBound(int[] arr, int n, int x){
	int low = 0;
	int high = n-1;
	int res = n; // hypothetical answer
	
	while (low <= high){
		int mid = low + (high - low) /2;
		if (arr[mid] >= x){
			res = mid;
			high = mid - 1; // look for more small index on left
		}
		else{
			low = mid + 1; // look to the right
		}
	}
	return res;
}
```



> TC: 2 * O(Log n) base 2,
> Sc: O(1)



## Find the number of occurrence of x in a sorted array
> Make sure to add the check if first occurrence is coming as -1, then return 0; else

> Last occurrence - First occurrence + 1


