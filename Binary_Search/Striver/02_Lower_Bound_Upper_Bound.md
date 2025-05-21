## Lower Bound

>Smallest index such that arr[ind] >= x

#### #LowerBound 
```java
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

>TC: O(Log n) base 2

---
## Upper Bound

>Smallest index such that arr[ind] > x

#### #UpperBound
```java
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
```

>TC: O(Log n) base 2

---
## Search Insert Position

>Lower bound is the answer of this problem statement
### Lower Bound

>Smallest index such that arr[ind] >= x

#### #LowerBound

```java
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

>TC: O(Log n) base 2

---
## Floor and Ciel in Sorted Array

> Floor --> Largest number in array <= x
> Ciel --> Smallest number in array >= x

>Ciel --> Just a lower_bound, but be careful about the case when Ciel is not present then probably they ask to return -1. ‼️

#### Example
```
arr = {10, 20, 30, 40, 50}

if x = 25, 
then
	Floor --> 20
	Ciel --> 30
```


#### For the Floor

##### Code
```java
public int findFloor(int[] arr, int n, int x){
	int low = 0;
	int high = n-1;
	int res = n; // hypothetical answer
	
	while (low <= high){
		int mid = low + (high - low) /2;
		if (arr[mid] <= x){
			res = mid;
			low = mid + 1; // look for more largest index/ele to the right
		}
		else{
			high = mid - 1; // look to the left
		}
	}
	return res;
}
```