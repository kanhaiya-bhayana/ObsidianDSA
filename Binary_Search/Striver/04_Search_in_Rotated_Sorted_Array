## Search in a sorted and rotated array

```
Key Points:
1. Identify the sorted half.
2. Implement the necessary checks
```



```java
public int findInSortedAndRotated(int[] arr, int n, int k){
  int low = 0;
  int high = n-1;

  while (low <= high){
    int mid = low + (high-low)/2;

    if (arr[mid] == k) return mid;
    
    // identify the left half
    if (arr[low] <= arr[mid]){ // this half is sorted
      if (arr[low] <= k && arr[mid] >= k){
        high = mid - 1;
      }
      else{
        low = mid +1;
      }
    }
    // right half will be sorted
    else{
      if (arr[mid] <= k && arr[high] >= k){
        low = mid + 1;
      }
      else{
        high = mid - 1;
      }
    }
  }
  return -1;
}
