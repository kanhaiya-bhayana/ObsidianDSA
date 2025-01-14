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

#### Wrapping around the alphabet when incrementing or decrementing a character.

###### For Incrementing 'a' to 'z':

When adding 1 to `'a'`, wrap around back to `'z'`. Use the modulus operator to achieve this.

###### For Decrementing 'z' to 'a':

Similarly, when subtracting 1 from `'z'`, wrap around to `'a'`.

```java
class Main {
    public static void main(String[] args) {
        char currentChar = 'a';

        // Increment and wrap around
        char nextChar = (char) ('a' + (currentChar - 'a' + 1) % 26);
        System.out.println("After incrementing 'a': " + nextChar);

        currentChar = 'z';
        
        // Decrement and wrap around
        char previousChar = (char) ('a' + (currentChar - 'a' - 1 + 26) % 26);
        System.out.println("After decrementing 'z': " + previousChar);
    }
}
```

