## Steps
1. Find the max
2. Create the count frequency array
3. Iterate over the count array and map the res

```java
private void countSort(int []arr)
    {
        // find max 
        int mx = Integer.MIN_VALUE;
        for (int n : arr)
        {
            mx = Math.max(n,mx);
        }

        int []count = new int[mx+1];

        for (int i = 0; i < arr.length; i++)
        {
            count[arr[i]]++;
        }

        int ind = 0;
        for (int i = 0; i < count.length; i++)
        {
            while (count[i] > 0)
            {
                arr[ind] = i;
                ind++;
                count[i]--;
            }
        }
    }
```

