
```java
//Back-end complete function Template for Java
class Solution {
    public static int findPages(int[] arr, int k) {
        // code here
        int res = -1;
        int n = arr.length;

        if (n < k)
            return res;

        int low = maxInArray(arr);
        int high = sumOfAll(arr);

        while (low <= high){
            int mid = low + (high-low) / 2;

            if (isValid(arr,mid,k)){
                res = mid;
                high = mid - 1;
            }
            else{
                low = mid + 1;
            }
        }

        return res;
    }
    
    private static boolean isValid(int[] arr, int mid, int k){
        int stu = 1;
        int sum = 0;

        for (int n : arr){
            sum +=n;
            if (sum > mid){
                stu += 1;
                sum = n;
                
                if (stu > k){
	                return false;
	            }
            }
        }
        return true;
    }

    private static int maxInArray(int[] arr){
        int max = Integer.MIN_VALUE;
        for (int n : arr){
            if (n > max){
                max = n;
            }
        }
        return max;
    }

    private static int sumOfAll(int[] arr){
        int sum = 0;
        for (int n : arr){
            sum += n;
        }
        return sum;
    }
}
```