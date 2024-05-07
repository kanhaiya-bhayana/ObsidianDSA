
```java
private ListNode convertStackToLL(Stack<Integer> st)
    {
        if (st.isEmpty())
        {
            return new ListNode();
        }
        ListNode head = new ListNode(st.pop());
        ListNode mover = head; 
        while (!st.isEmpty())
        {
            ListNode temp = new ListNode(st.pop());
            mover.next = temp;
            mover = temp;
        }
        return head;
    }
```


