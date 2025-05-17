## Q. Form the smallest number from single digits (Count Sort).

`Description`
### Find the smallest number that can be formed by rearranging the digits of the given number in an array. Return the smallest number in the form an array. #Sorting

#### Solution-1: Sorting

>Determine the frequency of each number in the array, and then reconstruct the array by placing each number back into the array as many times as its frequency.


##### Code
```java
public int[] solve(int[] arr){
	int n = arr.length;
	int[] freq = new int[10];

	for (int i=0; i<n; i++){
		freq[arr[i]]++;
	}
	
	int ind = 0;
	
	for (int i=0; i<=9; i++){
		int cnt = freq[i];
		
		for (int j=0; j<cnt; j++){
			arr[ind] = i;
			ind++;
		}
	}
	
	return arr;
}
```

>TC: O(n)
>SC: O(1)

## Count Sort on Large values

> if 0 <= A[i] <= 10^9


>Count sort is generally recommended when the 0 <= A[i] <= 10^6 , which is equivalent to 4 MB.

## Count Sort on Negative Numbers

```java
import java.util.Arrays;

public class CountSortNegative {
    public static int[] countSort(int[] arr) {
        // Step 1: Find the minimum and maximum values
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        
        for (int num : arr) {
            if (num < min) min = num;
            if (num > max) max = num;
        }
        
        // Step 2: Calculate the range and offset
        int offset = -min;
        int range = max - min + 1;
        
        // Step 3: Create and populate the count array
        int[] count = new int[range];
        for (int num : arr) {
            count[num + offset]++;
        }
        
        // Step 4: Reconstruct the sorted array
        int[] sortedArr = new int[arr.length];
        int index = 0;
        
        for (int i = 0; i < count.length; i++) {
            while (count[i] > 0) {
                sortedArr[index++] = i - offset;
                count[i]--;
            }
        }
        
        return sortedArr;
    }

    public static void main(String[] args) {
        int[] array = {-5, -10, 0, -3, 8, 5, -1, 10};
        int[] sortedArray = countSort(array);
        System.out.println("Sorted Array: " + Arrays.toString(sortedArray));
    }
}

```

## Q. Merge two sorted arrays
### Given an integer array where all odd elements are sorted and all even elements are sorted. Sort the entire array.

#### Soln. 1: Using sorting function

> TC: O(n log n)

#### Soln. 2

> Create 2 arrays for segregating odd and even elements
> Use 2 pointers and keep on each one of them, and start comparing, just like the merge function of the merge sort.

##### Code
```java
public int[] solve(int[] arr){
	int n = arr.length;
	int nE = 0; // for keeping the track of even elements
	int nO = 0; // for keeping the track of no. of odd elements
	
	for (int a : arr){
		if ((a&1) == 0){
			nE++;
		}
		else{
			nO++;
		}
	}
	
	int[] even = new int[nE];
	int[] odd = new int[nO];
	
	int e = 0;
	int o = 0;
	
	for (int a : arr){
		if ((a&1) == 0){
			even[e++] = a;
		}
		else{
			odd[o++] = a;
		}
	}
	
	e=0;
	o=0;
	int ind=0;
	
	while (e < nE && o < nO){
		if (even[e] < odd[o]){
			arr[ind++] = even[e++];
		}
		else{
			arr[ind++] = odd[o++];
		}
	}
	
	while (e < nE){
		arr[ind++] = even[e++];
	}
	
	while (o < nO){
		arr[ind++] = odd[o++];
	}
	
	return arr;
}
```


>TC: O(n)
>SC: O(n)