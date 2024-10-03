
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

##### List- Integer to int[]

```java
List<Integer> list = new ArrayList<>();

int[] arr = list.stream().mapToInt(Integer::intValue).toArray();
```


##### int to String

```java
int n = 89675;
StringBuilder s = new StringBuilder(String.valueOf(n));
```

##### String to int

```java
String s = "87";
int n = Integer.parseInt(n);
```
