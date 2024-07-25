
#### Array to List

```java
List<Integer> t = Arrays.stream(arr).boxed().collect(Collectors.toList());
```


```java
Integer[] arr = {2,3,4,4};  
List<Integer> list = new ArrayList<Integer>(Arrays.asList(arr));
```

#### int[] --> Integer[]

```java
int[] arr;
Integer[] integerArr = Arrays.stream(arr).boxed().toArray(Integer[]::new);
```
#### Integer[] --> int[]

```java
int[] arr = Arrays.stream(integerArr).mapToInt(Integer::intValue).toArray();
```

