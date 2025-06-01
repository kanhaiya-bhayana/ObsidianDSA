## What is Reflection?

>This is used to examine the Classes, Methods, Fields, Interfaces at runtime and also possible to change the behavior of the class too.

##### For Example
- What all methods present in the class.
- What all fields present in the class.
- What is the return type of the method.
- What is the modifier of the Class.



```java
public static void main(String[] args) {  

    Class<Eagle> eagle = Eagle.class;  
    System.out.println(eagle.getName());  
    Method[] methods = eagle.getMethods();  
    
    for (Method method : methods) {  
        System.out.println(method.getName());  
    }
}
```

> With the help of reflection we can access and set the values of a private field as well.


> Using reflection there is a disadvantage and biggest is, it breaks the singleton pattern.

> Breaks the OOPs principles.
> Reflection is slow, try all the things on the run time.


