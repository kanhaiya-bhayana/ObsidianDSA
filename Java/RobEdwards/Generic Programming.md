

Equals() method - compare the memory location of the 1 objects inside the heap, are they equal or not..



```java
Object []arr = new Objct[10];
Student s = new Student();
Object obj = s;
arr[0] = obj;


Monkey m = new Monkey();
arr[1] = m;

String str = "Chalk & Cheese";
arr[2] = str;
```

Yes, the above code will compile, bcz Student, Monkey and String are objects,
but
It's not a psychologically pleasing peace of code.

#### Solution
Parameterized Types
