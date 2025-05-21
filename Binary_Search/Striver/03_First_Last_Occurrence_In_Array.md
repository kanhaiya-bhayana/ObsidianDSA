## Find the first and last occurrence of x

> First --> lower bound
> Last --> upper bound 

! but make sure we have to handle the edge cases, like if lb == n or lb != x then return -1, i.e., ele does not exists.

```java

public int[] findFirstAndLast(int[] arr, int n, int x) {
    int lb = lowerBound(arr, n, x);
    int ub = upperBound(arr, n, x) - 1; // Convert upper bound to an inclusive index
    
    // Check if x is not found
    if (lb == n || arr[lb] != x) {
        return new int[]{-1, -1};
    }
    return new int[]{lb, ub};
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
