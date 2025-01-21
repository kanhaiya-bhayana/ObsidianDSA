

```java
public class Subsequence  
{  
    public void f(int ind,int []arr, List<Integer> ds)  
    {        // base case  
        if (ind >= arr.length)  
        {   printSubSeq(ds);  
            return;  
        }  
        ds.add(arr[ind]);  
        f(ind + 1, arr, ds); // take  
        ds.remove(ds.size()-1);  
        f(ind + 1, arr, ds); // not take  
    }  
  
    private void printSubSeq(List<Integer> ds) {  
        if (ds.isEmpty())  
        {            System.out.println("{}");  
            return;  
        }        for (int it : ds)  
        {            System.out.print(it +" ");  
        }        System.out.println();  
    }
}
```

