https://www.naukri.com/code360/problems/subset-sum_3843086?utm_source=striver&utm_medium=website&utm_campaign=a_zcoursetuf&leftPanelTabValue=PROBLEM

```java
import java.util.* ;

import java.io.*;

public class Solution {

    static ArrayList<Integer> res;

    public static ArrayList<Integer> subsetSum(int num[]) {

        // Write your code here..

        res = new ArrayList<>();

  

        dfs(0,0,num);

        Collections.sort(res);

        return res;

    }

  

    public static void dfs(int ind, int sum, int []arr)

    {

        // base case

        if (ind == arr.length)

        {

            res.add(sum);

            return;

        }

  

        sum+=arr[ind];

        dfs(ind+1,sum,arr);

        sum-=arr[ind];

        dfs(ind+1,sum,arr);

  

    }

  

}
```

