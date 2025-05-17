Define by start and end time
-> start <= end


#### Given some intervals, merge them if they intersect

#### Approach #Approach1 
```
1. Create an array to store the merged intervals
2. Start with 0th interval in your hand
3. From i = 1 to n - 1
	1. if ith interval is intersect:
			merge it with inhand interval and updat inhand interval
	2. else
			Add interval hand to the output, and take ith interval in hand
4.

Make sure given interval array is sorted

currS = arr[0][0];
currE = arr[0][1];

for (i = 1 to N - 1){
	if (arr[i][0] <= currE){
		currE = Math.max(currE, arr[i][1]);
	}
	else{
		ans.add({currS, currE});
		currS = arr[i][0];
		currE = arr[i][1];
	}
}
currE = Math.max(currE, arr[i][1]);
```


```
TC: O(N)
SC: O(1)
```

### LeetCode: 56 | [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        
        List<int[]> res = new ArrayList<>();

        if (intervals.length == 0 || intervals == null){
            return res.toArray(new int[0][]);
        }

        Arrays.sort(intervals, (a,b) -> Integer.compare(a[0],b[0]));

        int start = intervals[0][0];
        int end = intervals[0][1];

        for (int[] it : intervals){
            if (it[0] <= end){
                end = Math.max(end, it[1]);
            }
            else{
                res.add(new int[] {start, end});
                start = it[0];
                end = it[1];
            }
        }

        res.add(new int[]{start, end});
        return res.toArray(new int[res.size()][]);
    }
}
```