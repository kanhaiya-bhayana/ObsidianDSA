Allows us to define a container.

```java
MyContainer<Monkey> = new MyContainer<Monkey>():

MyContainer<Undergraduate> = new MyContainer<Undergraduate>():
```


## Generics

```java
public class LinkedList<T>{

}
```

```java
public void addFirst(T obj)
```

```java
public T removeFirst()
```



```java
class Node<T>
{
	T data;
	Node<T> next;
	
	// constructor
	public Node(T obj)
	{
		data = obj;
		next = null;
	}
}
```

### Create a Generic Array

```java
T[] array = (T[]) new Object[size];

T[] array = new T[size]; // this will not compile
```



```java
import java.util.ArrayList;
import java.util.List;

public class Main<T> {
    private List<T> list;

    public Main() {
        list = new ArrayList<>();
    }

    public List<T> getList() {
        return list;
    }

    public static void main(String[] args) {
        Main<String> main = new Main<>();
        List<String> list = main.getList();
        list.add("Hello");
        list.add("World");
        System.out.println("List size: " + list.size());
        System.out.println("Hello World");
    }
}
```

