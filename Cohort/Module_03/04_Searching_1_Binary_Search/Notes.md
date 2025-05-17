### #BinarySearch
### Q. Given a sorted array with integers search & find the inedex of a given element K If not present, return -1

##### Code
```java
public int binarySearch(int[] arr, int K){
	int s = 0;
	int e = arr.length - 1;
	
	while (s <= e){
		int mid = s + (e-s) /2 ;
		if (arr[mid] == K){
			return mid;
		}
		else if (arr[mid] > K){
			e = mid -1;
		}
		else{
			s = mid + 1;
		}
	}
	
	return -1;
}
```

> TC: O(Log n)
> SC: O(1)

### #BinarySearch
### Q. Given a sorted array of size N with duplicates. Find the index of first occurrence of a given target.

##### Code
```java
public int search(int[] arr, int k){
	int s = 0;
	int e = arr.length-1;
	
	while (s <= e){
		int mid = s + (e-s)/2;
		if (arr[mid] == k){
			if (mid == 0 || arr[mid-1] != arr[mid]){
				return mid;
			}
			else{
				e = mid - 1;
			}
		}
		else if (arr[mid] > k){
			e = mid - 1;
		}
		else{
			s = mid + 1;
		}
	}
	return -1;
}
```

>TC: O(Log n)
>SC: O(1)

### #BinarySearch
### Q. Find the last occurrence of a target in a sorted array.

##### Code
```java
public int search(int[] arr, int k){
	int s =0;
	int e = arr.length - 1;
	
	while (s <= e){
		int mid = s + (e-s) /2;
		if (arr[mid] == k){
			if (mid == n-1 || arr[mid] != arr[mid+1]){
				return mid;
			}
			else{
				s = mid + 1;
			}
		}
		else if (arr[mid] > k){
			e = mid - 1;
		}
		else{
			s = mid + 1;
		}
	}
	
	return -1;
}
```

>TC: O(Log n)
>SC: O(1)

### #BinarySearch 
### Find the frequency of just 1 element in the array, if the array is sorted

> We know how to find the first occurrence and same we can find the last occurrence
> so, 
> 		frequency = last - first  + 1


###  #LocalMinima
### Given an array of N distinct elements. Find any local minima in the array.
> Local minima => A no. which is smaller than it's adjacent neighbors.

