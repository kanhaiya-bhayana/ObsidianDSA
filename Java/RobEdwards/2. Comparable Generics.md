
- The Comparable interface is used to compare an object of the same class with an instance of that class, it provides ordering of data for objects of the user-defined class.
- The class has to implement the **java.lang.Comparable** interface to compare its instance, it provides the compareTo method that takes a parameter of the object of that class.

```java
Comparable<T> interface
public int compareTo(T obj)

a.compareTo(b);
if (a < b) return < 0;
if (a == b) return 0;
if (a > b) return > 0;



if (((Comparable<T>) data).compareTo(obj) == 0)
```



### Example 1

Given an array of Pairs consisting of two fields of type string and integer. you have to sort the array in ascending Lexicographical order and if two strings are the same sort it based on their integer value.

**Sample I/O:**

```java
Input:  { {"abc", 3}, {"a", 4}, {"bc", 5}, {"a", 2} }
Output:  { {"a", 2}, {"a", 4}, {"abc", 3}, {"bc", 5} }

Input:  { {"efg", 1}, {"gfg", 1}, {"cba", 1}, {"zaa", 1} }
Output:  { {"cba", 1}, {"efg", 1}, {"gfg", 1}, {"zaa", 1} }
```

```java


import java.io.*;
import java.util.*;
 
class Pair implements Comparable<Pair> {
    String x;
    int y;
 
    public Pair(String x, int y)
    {
        this.x = x;
        this.y = y;
    }
 
    public String toString()
    {
        return "(" + x + "," + y + ")";
    }
 
    @Override public int compareTo(Pair a)
    {
        // if the string are not equal
        if (this.x.compareTo(a.x) != 0) {
            return this.x.compareTo(a.x);
        }
        else {
            // we compare int values
            // if the strings are equal
            return this.y - a.y;
        }
    }
}
 
public class GFG {
    public static void main(String[] args)
    {
 
        int n = 4;
        Pair arr[] = new Pair[n];
 
        arr[0] = new Pair("abc", 3);
        arr[1] = new Pair("a", 4);
        arr[2] = new Pair("bc", 5);
        arr[3] = new Pair("a", 2);
 
        // Sorting the array
        Arrays.sort(arr);
 
        // printing the
        // Pair array
        print(arr);
    }
 
    public static void print(Pair[] arr)
    {
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
}

```


##### Output
```java
**Before Sorting:**
(abc, 3);
(a, 4);
(bc, 5);
(a, 2);

**After Sorting:**
(a,2)
(a,4)
(abc,3)
(bc,5)
```
