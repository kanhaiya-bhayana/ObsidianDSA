

### Print all Subsets where sum equal to k

```java
f(i, ds, s)
{
	if (i == n)
	{
		if (s == k)
		{
			print(ds);
		}
		return;
	}
	
	ds.add(arr[i]);
	f(i + 1, ds, s + arr[i]);  // take
	
	ds.remove(arr[i]);
	f(i + 1, ds, s);  // not take
}
```


```java
public void f(int ind,int []arr, List<Integer> ds, int sum, int k)  
    {        // base case  
        if (ind == arr.length)  
        {            if (sum == k)  
            {                printSubSeq(ds);  
            }            return;  
        }  
        ds.add(arr[ind]);  
        sum += arr[ind];  
        f(ind + 1, arr, ds, sum, k); // take  
  
        ds.remove(ds.size()-1);  
        sum -= arr[ind];  
        f(ind + 1, arr, ds, sum, k); // not take  
    }  
  
    private void printSubSeq(List<Integer> ds) {  
        if (ds.isEmpty())  
        {            System.out.println("{}");  
            return;  
        }        for (int it : ds)  
        {            System.out.print(it +" ");  
        }        System.out.println();  
    }}
```

### Print only 1 sequence where sum is k
- The technique to print answer is, in the base case if the condition is satisfied return true, else return false;

```java
if (f() == true)
{
	return true;
}

f();

return false;
```

```java
public boolean f(int ind,int []arr, List<Integer> ds, int sum, int k)  
{  
    // base case  
    if (ind == arr.length)  
    {        // condition satisfied  
        if (sum == k)  
        {            printSubSeq(ds);  
            return true;  
        }        // condition not satisfied  
        return false;  
    }  
    ds.add(arr[ind]);  
    sum += arr[ind];  
    if(f(ind + 1, arr, ds, sum, k) == true) // take  
    {  
        return true;  
    }  
    ds.remove(ds.size()-1);  
    sum -= arr[ind];  
    if (f(ind + 1, arr, ds, sum, k)) // not take  
    {  
        return true;  
    }    return false;  
}
```


### Count the subsequence where sum is equal to K

```java
int f()
{
	base case
		return 1 --> condition satisfies
		return 0 --> condition not satisfies
	l = f();
	r = f();
	return l + r;
}
```

```java
public int countSubsequenceSumK(int ind, int []arr, int sum, int k)  
{  
    // base case  
    if (ind == arr.length)  
    {        if (sum == k)  
        {            return 1;  
        }        return 0;  
    }  
    sum += arr[ind];  
    int take = countSubsequenceSumK(ind + 1, arr, sum, k);  
  
    sum -= arr[ind];  
    int notTake = countSubsequenceSumK(ind + 1, arr, sum, k);  
  
    return take + notTake;  
}
```
