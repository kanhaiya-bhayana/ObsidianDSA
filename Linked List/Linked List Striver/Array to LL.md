

```java
public Node convertArr2LL(int []arr){
        Node head = new Node(arr[0]);
        Node mover = head;
        for (int i = 1; i< arr.length; i++){
            Node temp = new Node(arr[i]);
            mover.next = temp;
            mover = temp; // you can also do like mover = mover.next;
        }
        return head;
    }
```