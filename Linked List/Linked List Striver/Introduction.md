#### Linked List Bootcamp

What is Linked List?
	A linked list is a linear data structure, in which the elements are not stored at contiguous memory locations. The elements in a linked list are linked using pointers as shown in the below image:

![[Linked-List-Data-Structure 1.png]]

```
class Node{
	int data;
	Node next;
	Node (int d){
		this.data = d;
		this.next = null;
	}
	Node (int d, Node next1){
		this.data = d;
		this.next = next1;
	}
}
```
#### Important terms
- Traversal in LL - O(n)
- Length of LL - O(n)
- Search an element in LL - O(n)
- Delete a node in LL - O(n)
- Insert a node in a LL - O(n)
#### Function to Find k-th Node

```java
public class LinkedListUtils {

    public static Node getKthNode(Node head, int k) {
        int count = 0;
        Node current = head;

        while (current != null && count < k) {
            current = current.next;
            count++;
        }

        // If k is out of bounds
        if (current == null) {
            return null;
        }

        return current;
    }
}

```
#### Convert an Array to Linked List
```java
import org.w3c.dom.Node;

/**
 * ConvertArrayToLL
 */
public class SinglyLinkedList {

    class Node{
        int data;
        Node next;

        Node(int d){
            this.data = d;
            this.next = null;
        }
        Node(int d, Node n){
            this.data = d;
            this.next = n;
        }

    }

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
    // do not lose the head, always use a copy of head
    public void printList(Node head){
        if (head == null) System.out.println("List is empty!");
        Node currNode = head;
        while (currNode != null) {
            System.out.print(currNode.data + " --> ");
            currNode = currNode.next;
        }
        System.out.println("null");
    }

    // get the length of the linked list
    public int lenghtOfLL(Node head){
        int cnt = 0;
        Node curr = head;
        while (curr != null) {
            curr = curr.next;
            cnt++;
        }
        return cnt;
    }

    // search in a LL
    public boolean chekcIfPresent(Node head, int val){
        Node curr = head;

        while (curr != null) {
            if (curr.data == val) return true;
            curr = curr.next;
        }
        return false;
    }

    // delete the head of the LL
    public Node removeHead(Node head){
        if (head == null) return head;
        head = head.next;
        return head;
    }

    // delete the last of the LL
    public int removeLast(Node head){
        if (head == null || head.next == null) return 0;

        // stop at the 2nd last element
        Node curr = head;
        while (curr.next.next != null) {
            curr = curr.next;
        }
        int deletedNode = curr.next.data;
        curr.next = null;
        return deletedNode;
    }

    // remove the kth element from the LL
    public Node removeK(Node head, int k){
        if (head == null) return head;
        Node curr = head;
        if (k == 1){
            curr = curr.next;
            return curr;
        }
        Node prev = null;
        int count = 0;
        // now for handling remaining k's
        while (curr != null) {
            count++;

            if (count == k){
                prev.next = prev.next.next;
                break;
            }
            prev = curr;
            curr = curr.next;
        }
        return head;
    }

    // remove the specific element from the LL
    public Node removeEle(Node head, int ele){
        if (head == null) return head;
        Node curr = head;
        if (curr.data == ele){
            curr = curr.next;
            return curr;
        }
        Node prev = null;
        while (curr != null) {
            if (curr.data == ele){
                prev.next = prev.next.next;
                break;
            }
            prev = curr;
            curr = curr.next;
        }
        return head;
    }

    // insert the element at the begining
    public Node insertHead(Node head, int val){
        return new Node(val,head);
    }

    // insert at the end of the LL
    public Node insertTail(Node head, int val){

        if (head == null){
            return new Node(val);
        }
        Node curr = head;
        while (curr.next != null) {
            curr = curr.next;
        }
        Node newNode = new Node(val);
        curr.next = newNode;
        return head;
    }

    // insert at the kth Position
    public Node insertAtPosition(Node head,int val, int k){
        if (head == null){
            if (k == 1){
                return new Node(val);
            }
            return head;
        }

        if (k ==1){
            Node newNode = new Node(val,head);
            return newNode;
        }

        int cnt = 0;
        Node curr = head;
        while (curr != null) {
            cnt++;
            if (cnt == k-1){
                Node newNode = new Node(val);
                newNode.next = curr.next;
                curr.next = newNode;
                break;
            }
            curr = curr.next;
        }

        return head;
    }

    // insert ele before value
    public Node insertBeforeValue(Node head,int ele, int val){
        if (head == null){
            return head;
        }
        if (head.data == val){
            return new Node(ele,head);
        }
        Node curr = head;
        while (curr.next != null) {
            if (curr.next.data == val){
                Node newNode = new Node(ele);
                newNode.next = curr.next;
                curr.next = newNode;
                break;
            }
            curr = curr.next;
        }

        return head;
    }

	public void reverseIterate(){
        if (head == null || head.next == null){
            return;
        }
        Node curr = head;
        Node prev = null;
        while (curr != null) {
            Node next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        head = prev;
    }

    public static void main(String[] args) {
        SinglyLinkedList ll = new SinglyLinkedList();
        int []arr = {12, 5, 6, 8};
        Node head = ll.convertArr2LL(arr);
        // System.out.println(head.data);
        ll.printList(head);
        //     System.out.println(ll.lenghtOfLL(head));
        //     System.out.println(ll.chekcIfPresent(head, 5));
        //     head = ll.removeHead(head);
        //     ll.printList(head);
        //     System.out.println(ll.removeLast(head));
        // head = ll.removeEle(head,6);
        // head = ll.insertHead(head, 100);
        // head = ll.insertTail(head, 200);
        head = ll.insertBeforeValue(head, 100,6);
        ll.printList(head);

    }
}
```


#### Doubly Linked List
- A **doubly linked list** (DLL) is a special type of linked list in which each node contains a pointer to the previous node as well as the next node of the linked list.

![[DLL1.png]]

#### DLL Node Structure
```
class Node{
	int data;
	Node next;
	Node back;
	Node (int d){
		this.data = d;
		this.next = null;
		this.back = null;
	}
	Node (int d, Node next1, Node back1){
		this.data = d;
		this.next = next1;
		this.back = back1;
	}
}
```


#### Doubly Linked List Implementation
```java
/**  
 * DLL */public class DLL {  
  
    class Node{  
        int data;  
        Node back;  
        Node next;  
  
        Node(int d){  
            this.data = d;  
            this.back = null;  
            this.next = null;  
        }  
  
        Node(int d, Node b, Node n){  
            this.data = d;  
            this.back = b;  
            this.next = n;  
        }  
    }  
  
    // convert the Array to Doubly Linked List  
    public Node convertArr2DLL(int []arr){  
        Node head = new Node(arr[0]);  
        Node prev = head;  
        for (int i = 1; i< arr.length; i++){  
            Node temp = new Node(arr[i], prev, null);  
            prev.next = temp;  
            prev = temp; // you can also do like prev = prev.next;  
        }  
        return head;  
    }  
  
    // print the Linked list  
    public void printList(Node head){  
        if (head == null) System.out.println("List is empty!");  
        Node currNode = head;  
        while (currNode != null) {  
            System.out.print(currNode.data + " --> ");  
            currNode = currNode.next;  
        }  
        System.out.println("null");  
    }  
  
    // delete the head of the DLL  
    public Node deleteHead(Node head){  
        if (head == null || head.next == null){  
            return null;  
        }  
  
        Node prev = head;  
        head = head.next;  
  
        head.back = null;  
        prev.next = null;  
  
        return head;  
    }  
  
    // delete the tail of the DLL  
    public Node deleteTail(Node head){  
        if (head == null || head.next == null) return null;  
  
        // Node prev = head;  
        Node curr = head;  
  
        while (curr.next != null) {  
            // prev = curr;  
            curr = curr.next;  
        }  
        // prev.next = null;  
        Node newTail = curr.back;  
        newTail.next = null;  
        curr.back = null;  
  
        return head;  
    }  
  
    // delete the kth element  
    public Node deleteKthEle(Node head, int k){  
        if (head == null) return null;  
  
        Node curr = head;  
        int cnt = 0;  
  
        while (curr != null) {  
            cnt++;  
  
            if (cnt == k) break;  
  
            curr = curr.next;  
        }  
  
        // after breaking the loop we have to extract the prev and next from the curr...  
        Node prev = curr.back;  
        Node next = curr.next;  
  
        if (prev == null && next == null){  
            return null;  
        }  
  
        else if (prev == null){  
            return deleteHead(head);  
        }  
  
        else if (next == null){  
            return deleteTail(head);  
        }  
  
        // now remove the kth element  
        prev.next = next;  
        next.back = prev;  
  
        curr.back = null;  
        curr.next = null;  
  
        return head;  
    }  
  
    // delete the node from the linked list EXCEPT HEAD  
    public void deleteNode (Node temp){  
        Node prev = temp.back;  
        Node next = temp.next;  
  
        if (next == null){  
            prev.next = null;  
            temp.back = null;  
            return;  
        }  
        prev.next = next;  
        next.back = prev;  
  
        temp.back = null;  
        temp.next = null;  
    }  
  
    // insert before head  
    public Node insertBeforeHead(Node head, int val){  
        Node newHead = new Node(val,null,head);  
        head.back = newHead;  
  
        return newHead;  
    }  
  
    public Node insertBeforeTail(Node head, int val){  
        if (head.next == null){  
            return insertBeforeHead(head, val);  
        }  
  
        Node curr = head;  
        while (curr.next != null) {  
            curr = curr.next;  
        }  
  
        Node prev = curr.back;  
        Node newNode = new Node(val,prev,curr);  
        prev.next = newNode;  
        curr.back = newNode;  
        return head;  
    }  
  
    // insert before the kth element  
    public Node insertBeforeKthEle(Node head, int k, int val){  
        if (k == 1){  
            return insertBeforeHead(head, val);  
        }  
  
        Node curr = head;  
        int cnt = 0;  
        while (curr.next != null) {  
            cnt++;  
            if (cnt == k){  
                break;  
            }  
            curr = curr.next;  
        }  
        Node prev = curr.back;  
        Node newNode = new Node(val,prev,curr);  
        prev.next = newNode;  
        curr.back = newNode;  
  
        return head;  
    }  
  
    // insert before the given node  
    public void insertBeforeNode(Node temp, int val){  
        Node prev = temp.back;  
        Node newNode = new Node(val, prev,temp);  
        prev.next = newNode;  
        temp.back = newNode;  
    }  
  
    // main  
    public static void main(String[] args) {  
        DLL ll = new DLL();  
        int []arr = {12, 5, 6, 8};  
        Node head = ll.convertArr2DLL(arr);  
        // System.out.println(head.data);  
        ll.printList(head);  
        // ll.deleteNode(head.next.next);  
        ll.insertBeforeNode(head.next.next.next, 500);  
        ll.printList(head);  
    }  
}
```


#### Reverse a DLL
```java
// Reverse Doubly LL  
public Node reverseIterate(Node head)
{  
    if (head == null || head.next == null){  
        return null;  
    }  
    Node curr = head;  
    Node prev = null;  
  
    while (curr != null) {  
        prev = curr.back;  
        curr.back = curr.next;  
        curr.next = prev;  
        curr = curr.back;  
    }  
    return prev.back;  
}
```
#### Add Two Numbers

[Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
	
```java
/**  
 * Definition for singly-linked list. * public class ListNode { *     int val; *     ListNode next; *     ListNode() {} *     ListNode(int val) { this.val = val; } *     ListNode(int val, ListNode next) { this.val = val; this.next = next; } * } */
 * 
 class Solution {  
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {  
        ListNode t1 = l1;  
        ListNode t2 = l2;  
  
        ListNode dummyNode = new ListNode(-1);  
        ListNode curr = dummyNode;  
        int carry = 0;  
        int sum = 0;  
        while (t1 != null || t2 != null){  
            sum = carry;  
            if (t1 != null){  
                sum = sum + t1.val;  
            }  
            if (t2 != null){  
                sum = sum + t2.val;  
            }  
  
            // create newNode for storing the result of sum  
            ListNode newNode = new ListNode(sum % 10);  
            carry = sum / 10;  
  
            curr.next = newNode;  
            curr = curr.next;  
  
            if (t1 != null) t1 = t1.next;  
            if (t2 != null) t2 = t2.next;  
        }  
  
        // if carry is left then create a new node for that  
        if (carry > 0){  
            ListNode newNode = new ListNode(carry);  
            curr.next = newNode;  
        }  
        return dummyNode.next;  
    }  
}
```
#### Odd & Even Linked List
- You have to group the odd and even index data and return the head of the list
https://leetcode.com/problems/odd-even-linked-list/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode oddEvenList(ListNode head) {
        
        if (head == null || head.next == null) return head;
        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenHead = head.next;

        while (even != null && even.next != null){
            odd.next = odd.next.next;
            even.next = even.next.next;

            odd = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```

#### Sort a LinkedList of 0's, 1's and 2's | Multiple Approaches
https://www.geeksforgeeks.org/problems/given-a-linked-list-of-0s-1s-and-2s-sort-it/1

```java
/*
class Node
{
    int data;
    Node next;
    Node(int data)
    {
        this.data = data;
        next = null;
    }
}
*/
class Solution
{
    //Function to sort a linked list of 0s, 1s and 2s.
    static Node segregate(Node head)
    {
        
        if (head == null || head.next == null) return head;
        // add your code here
        Node zeroHead = new Node(-1);
        Node oneHead = new Node(-1);
        Node twoHead = new Node(-1);
        
        Node zero = zeroHead;
        Node one = oneHead;
        Node two = twoHead;
        
        Node temp = head;
        
        while (temp != null){
            if (temp.data == 0){
                zero.next = temp;
                zero=temp;
            }
            
            else if (temp.data == 1){
                one.next = temp;
                one = temp;
            }
            else{
                two.next = temp;
                two = temp;
            }
            temp = temp.next;
        }
        
        zero.next = (oneHead.next != null) ? oneHead.next : twoHead.next;
        one.next = twoHead.next;
        two.next = null;
        
        return zeroHead.next;
        
    }
}
```
#### Nth node from end of linked list
https://www.geeksforgeeks.org/problems/nth-node-from-end-of-linked-list/1

```java
/* Structure of node

class Node
{
    int data;
    Node next;
    Node(int d) {data = d; next = null; }
}
*/

class Solution
{
    //Function to find the data of nth node from the end of a linked list.
    int getNthFromLast(Node head, int n)
    {
    	// Your code here	
    	
    	Node fastP = head;
    	Node slowP = head;
    	int cnt = 0;
    	Node temp = head;
    	while (temp != null){
    	    temp = temp.next;
    	    cnt++;
    	}
    	
    	if (cnt < n) return -1;
    	else if (n == cnt) return head.data;
    	
    	for (int i = 0; i< n ; i++){
    	    fastP = fastP.next;
    	}
    	
    	if (fastP == null){
    	    return head.next.data;
    	}
    	
    	while (fastP.next != null){
    	    slowP = slowP.next;
    	    fastP = fastP.next;
    	}
    	
    	
    	
    	Node delNode = slowP.next;
    	slowP.next = slowP.next.next;
    	int res = delNode.data;
    	delNode = null;
    	
    	return res;
    }
}
```
#### Reverse Using Recursion
https://leetcode.com/problems/reverse-linked-list/description/
```java
private ListNode recursiveReverse(ListNode head){
        if (head == null || head.next == null){
            return head;
        }
        ListNode newNode = recursiveReverse(head.next);
        ListNode front = head.next;
        front.next = head;
        head.next = null;
        return newNode;
    }
```
#### LinkedList is Palindrome or not
https://leetcode.com/problems/palindrome-linked-list/description/
```java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        return checkPalindrome(head);
    }
    boolean checkPalindrome(ListNode head){   

        // 3 step 
        // 1 - find middle
        // 2 - reverse the 2nd half
        // 3 - compare

        // 1- find middle using TURTOISE algo - slow and fast pointers
        ListNode slow = head;
        ListNode fast = head;

        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // 2- reverse the 2nd half
        ListNode newHead = reverseRecursive(slow.next);

        ListNode first = head;
        ListNode second = newHead;

        // 3 compare the first half and second half
        while (second != null) {
            if (first.val != second.val){
                reverseRecursive(newHead); // important after checking make everything to the original
                return false;
            }
            first = first.next;
            second = second.next;
        }
        reverseRecursive(newHead); // important after checking make everything to the original 
        return true;
    }

    private ListNode reverseRecursive(ListNode head){
        // base case
        if (head == null || head.next == null) return head;

        ListNode newHead = reverseRecursive(head.next);
        ListNode front = head.next;
        front.next = head;
        head.next = null;

        return newHead;
    }
}
```
#### Add 1 to a number represented as linked list
https://www.geeksforgeeks.org/problems/add-1-to-a-number-represented-as-linked-list/1

```java
/*
class Node{
    int data;
    Node next;
    
    Node(int x){
        data = x;
        next = null;
    }
} 
*/

class Solution
{
    public static Node addOne(Node head) 
    { 
        //code here.
        return solve(head);
        
    }
    
    public static Node solve(Node head){
        int carry = helper(head);
        
        if (carry == 1){
            Node newNode = new Node(1);
            newNode.next = head;
            return newNode;
        }
        
        return head;
    }
    
    public static int helper(Node temp){
        if (temp == null){
            return 1;
        }
        int carry = helper(temp.next);
        temp.data = temp.data + carry;
        
        if (temp.data < 10) return 0;
        
        temp.data = 0;
        return 1;
    }
}
```

#### Intersection of Two Linked Lists 
https://leetcode.com/problems/intersection-of-two-linked-lists/description/

```java
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        
        ListNode t1 = headA;
        ListNode t2 = headB;

        if (t1 == null || t2 == null) return null;

        while (t1 != t2){

            t1 = t1.next;
            t2 = t2.next;

            if (t1 == t2) return t1;

            if (t1 == null) t1 = headB;
            if (t2 == null) t2 = headA;
        }
        return t1;
    }
}
```




#### Find the Middle element of the Linked List

	(n/2) + 1
	
	https://leetcode.com/problems/middle-of-the-linked-list/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {

        if (head == null || head.next == null) return head;

        // tortoise algorithm
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```
#### Detect a loop or cycle in the Linked List
	Tortoise & Hare algorithm
	
	https://leetcode.com/problems/linked-list-cycle/description/

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) return true;
        }
        return false;

    }
}
```



#### Find the length of the Loop in LinkedList
https://www.geeksforgeeks.org/problems/find-length-of-loop/1

```java
/*
class Node
{
    int data;
    Node next;
    Node(int d) {data = d; next = null; }
}
*/
//Function should return the length of the loop in LL.
class Solution
{
    //Function to find the length of a loop in the linked list.
    static int countNodesinLoop(Node head)
    {
        //Add your code here.
        if (head == null || head.next == null) return 0;

        Node slow = head;
        Node fast = head;

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) return lengthLoop(slow, fast);
        }
        return 0;
    }
    
    private static int lengthLoop(Node slow, Node fast){
        int cnt = 1;
        fast = fast.next;
        
        while (slow != fast){
            cnt++;
            fast = fast.next;
        }
        return cnt;
    }
}
```
#### Delete the middle node of the Linked List
	Tortoise algo variation
	https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/description/
	
```java
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        
        if (head == null || head.next == null) return head;

        ListNode slow = head;
        ListNode fast = head;

        fast = fast.next.next;

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        slow.next = slow.next.next;
        return head;
    }
}
```
#### Find the starting point of the Loop/Cycle in LinkedList
	Tortoise & Hare algorith
https://leetcode.com/problems/linked-list-cycle-ii/description/

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        //Add your code here.
        if (head == null || head.next == null) return null;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast) return startingPoint(head,fast);
        }
        return null;
    }
    private ListNode startingPoint(ListNode slow, ListNode fast){
        
        while (slow != fast){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```
#### Delete all occurrences of a key in DLL
https://www.geeksforgeeks.org/problems/delete-all-occurrences-of-a-given-key-in-a-doubly-linked-list/1

```java
/* Structure of Doubly Linked List
class Node
{
	int data;
	Node next;
	Node prev;
	Node(int data)
	{
	    this.data = data;
	    next = prev = null;
	}
}*/
class Solution {
    static Node deleteAllOccurOfX(Node head, int x) {
        // Write your code here
        Node temp = head;
        
        while (temp != null){
            
            if (temp.data == x){
                // if this is the head of the LL, then post deletion the head will be updated
                if (temp == head){
                    head = temp.next;
                }
                
                Node nextNode = temp.next;
                Node prevNode = temp.prev;
                
                if (nextNode != null) nextNode.prev = prevNode;
                if (prevNode != null) prevNode.next = nextNode;
                
                temp = nextNode;
            }
            else temp = temp.next;
        }
        
        return head;
    }
}
```
#### Delete all occurrences of a key in LL
https://leetcode.com/problems/remove-linked-list-elements/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        
        // your code here

        if (head == null) return head;
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode temp = dummy;


        while (temp.next != null){
            if (temp.next.val == val){                
                temp.next = temp.next.next;
            }
            else{
                temp = temp.next;
            }
        }
        return dummy.next;
    }
}
```
#### Find all Pairs with given Sum in DLL
https://www.geeksforgeeks.org/problems/find-pairs-with-given-sum-in-doubly-linked-list/1

```java
/*

Definition for singly Link List Node
class Node
{
    int data;
    Node next,prev;
    
    Node(int x){
        data = x;
        next = null;
        prev = null;
    }
}

You can also use the following for printing the link list.
Node.printList(Node node);
*/

class Solution {
    public static ArrayList<ArrayList<Integer>> findPairsWithGivenSum(int target, Node head) {
        // code here
        
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        
        Node left = head;
        Node right = findTail(head);
        
        while (left.data < right.data){
            
            if (left.data + right.data == target){
                ArrayList<Integer> temp = new ArrayList<>();
                temp.add(left.data);
                temp.add(right.data);
                res.add(temp);
                left = left.next;
                right = right.prev;
            }
            else if (left.data + right.data < target){
                left = left.next;
            }
            else right = right.prev;
        }
        return res;
    }
    
    private static Node findTail(Node head){
        Node tail = head;
        
        while (tail.next != null) tail = tail.next;
        
        return tail;
    }
}
```
#### Remove duplicates from sorted LL
https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        
        if (head == null || head.next == null) return head;

        ListNode temp = head;

        while (temp != null && temp.next != null){
            ListNode nextNode = temp.next;

            while (nextNode != null && nextNode.val == temp.val){
                nextNode = nextNode.next;
            }
            temp.next = nextNode;
            temp = temp.next;
        }

        return head;
    }
}
```
#### Remove duplicates from sorted DLL
```java
/********************************************************

    Following is the class structure of the Node class:
    
    Definition of doubly linked list:
 * class Node {
 * public:
 *      int data;
 *      Node *prev;
 *      Node *next;
 *      Node() {
 *          this->data = 0;
 *          this->prev = NULL;
 *          this->next = NULL;
 *      }
 *      Node(int data) {
 *          this->data = data;
 *          this->prev = NULL;
 *          this->next = NULL;
 *      }
 *      Node (int data, Node *next, Node *prev) {
 *          this->data = data;
 *          this->prev = prev;
 *          this->next = next;
 *      }
 * };

********************************************************/

public class Solution {
    public static Node uniqueSortedList(Node head) {
        // Write your code here.
        // Write your code here.
        if (head == null || head.next == null) return head;

        Node temp = head;

        while (temp != null && temp.next != null){
            Node nextNode = temp.next;

            while (nextNode != null && nextNode.val == temp.val){
                nextNode = nextNode.next;
            }
            temp.next = nextNode;
            if (nextNode != null) nextNode.prev = temp;
            temp = temp.next;
        }

        return head;
    }
}
```


#### Reverse Nodes in K Group Size of LinkedList
https://leetcode.com/problems/reverse-nodes-in-k-group/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        
        ListNode temp = head;
        ListNode prevLast = null;

        while (temp != null){

            ListNode kthNode = getKthNode (temp,k);
            if (kthNode == null){
                if (prevLast != null) prevLast.next = temp;
                break;
            }

            ListNode nextNode = kthNode.next;
            kthNode.next = null; // break the list here ...
            reverseLinkedList(temp);

            if (temp == head) head = kthNode; // update the head ... reverseList
            else prevLast.next = kthNode; // connect to the next of the List ...

            prevLast = temp; 
            temp = nextNode;
        }
        return head;
    }

    private ListNode getKthNode(ListNode temp, int k){

        k = k-1;
        while (temp != null && k > 0){
            k--;
            temp = temp.next;
        }
        return temp;
    }

    private ListNode reverseLinkedList(ListNode head){
        if (head == null || head.next == null){
            return head;
        }
        ListNode newNode = reverseLinkedList(head.next);
        ListNode front = head.next;
        front.next = head;
        head.next = null;
        return newNode;
    }
}
```
#### Rotate a LinkedList
	if k == lenght | not do anything
	if k is multiple of length then also not do anything
	if (k % length == 0) return head;
	
	if k is larger then turn it down into smaller value by | k % length

https://leetcode.com/problems/rotate-list/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        
        // base case
        if (head == null || k == 0) return head;

        ListNode tail = head;
        int len = 1;
        while (tail.next != null){
            tail = tail.next;
            len++;
        }

        if (k % len == 0) return head;

        // make the k smaller
        k = k % len;

        // attach the tail to the head node
        tail.next = head;

        // find the nth node and update that next to the null but
        // before that point their next to the head
        ListNode newLastNode = findNthNode(head, len - k);
        head = newLastNode.next;
        newLastNode.next = null;

        return head;
    }

    private ListNode findNthNode(ListNode temp, int k){

        int cnt = 1;
        while (temp != null){
            if (cnt == k){
                return temp;
            }
            cnt++;
            temp = temp.next;
        }
        return temp;
    }
}
```
#### Merge two Sorted Linked Lists
https://leetcode.com/problems/merge-two-sorted-lists/description/
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        
        if (list1 == null) return list2;
        else if (list2 == null) return list1;

        ListNode t1 = list1;
        ListNode t2 = list2;

        ListNode dummyNode = new ListNode(-1);
        ListNode temp = dummyNode;

        while (t1 != null && t2 != null){

            if (t1.val < t2.val){
                temp.next = t1;
                temp = t1;
                t1 = t1.next;
            }
            else{
                temp.next = t2;
                temp = t2;
                t2 = t2.next;
            }
        }
        if (t1 != null) temp.next = t1;
        else temp.next = t2;

        return dummyNode.next;
    }
}
```
#### Flatten a Linked List
	Put all them into the array and sort the array and then convert an array to LL
	TC ~ O(2NM)
	
	https://www.geeksforgeeks.org/problems/flattening-a-linked-list/1

```java
/*Node class  used in the program
class Node
{
	int data;
	Node next;
	Node bottom;
	
	Node(int d)
	{
		data = d;
		next = null;
		bottom = null;
	}
}
*/
/*  Function which returns the  root of 
    the flattened linked list. */
class GfG
{
    Node flatten(Node root)
    {
	// Your code here
	return flattenList(root);
    }
    
    public static Node flattenList(Node head){
        
        if (head == null || head.next == null){
            return head;
        }
        
        Node mergeHead = flattenList(head.next);
        return mergeList(head, mergeHead);
    }
    
    public static Node mergeList(Node list1, Node list2){
        
        Node dummyNode = new Node(-1);
        Node res = dummyNode;
        
        while (list1 != null && list2 != null){
            if (list1.data < list2.data){
                res.bottom = list1;
                res = list1;
                list1 = list1.bottom;
            }
            else{
                res.bottom = list2;
                res = list2;
                list2 = list2.bottom;
            }
            res.next = null;
        }
        if (list1 != null) res.bottom = list1;
        else res.bottom = list2;
        
        return dummyNode.bottom;
    }
}
```
#### Merge K Sorted Lists
	implement using min-heap PriorityQueue
	https://leetcode.com/problems/merge-k-sorted-lists/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 * int val;
 * ListNode next;
 * ListNode() {}
 * ListNode(int val) { this.val = val; }
 * ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {

        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        Queue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
        for (ListNode list : lists)
            if (list != null)
                pq.offer(list);
        while (!pq.isEmpty()) {
            ListNode temp = pq.poll();
            if (temp.next != null)
                pq.offer(temp.next);
            cur.next = temp;
            cur = cur.next;
        }
        return dummy.next;

    }
}
```
#### Sort a Linked List
1. Brute Force
	1. Put all the elements into the array
	2. Sort the array
	3. Put back all elements into the LL
2. Use Merge Sort
	1. ![[Pasted image 20240215101152.png]]

https://leetcode.com/problems/sort-list/description/

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        
        // base case 
        if (head == null || head.next == null) return head;

        ListNode middle = findMiddle(head);
        ListNode leftHead = head;
        ListNode rightHead = middle.next;
        middle.next = null;

        leftHead = sortList(leftHead);
        rightHead = sortList(rightHead);

        return mergeTwoLists(leftHead, rightHead);
    }

    private ListNode mergeTwoLists(ListNode list1, ListNode list2){
        ListNode dummyNode = new ListNode(-1);
        ListNode temp = dummyNode;

        while (list1 != null && list2 != null){
            if (list1.val < list2.val){
                temp.next = list1;
                temp = list1;
                list1 = list1.next;
            }
            else{
                temp.next = list2;
                temp = list2;
                list2 = list2.next;
            }
        }
        if (list1 != null) temp.next = list1;
        else temp.next = list2;

        return dummyNode.next;
    }

    private ListNode findMiddle(ListNode head){
        ListNode temp = head;

        ListNode slow = head;
        ListNode fast = head.next; // in Tortoise we initialize the fast with the head but here we update

        while (fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```

#### Clone a LL with Next & Random Pointers
	1. Insert copy nodes in b/w
	2. Connecnt random pointers
	3. Connect next pointers 

![[Pasted image 20240216110333.png]]

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        
        insertCopyInBetween(head);
        connectRandomPointers(head);
        return getDeepCopyList(head);
    }

    private void insertCopyInBetween(Node head){

        // Node dummyNode = new Node(-1);
        Node temp = head;
        while (temp != null){
            Node copyNode = new Node(temp.val);
            Node nextEle = temp.next;
            copyNode.next = nextEle;
            temp.next = copyNode;
            temp = nextEle;
        }
    }

    private void connectRandomPointers(Node head){
        
        Node temp = head;
        while (temp != null){
            Node copyNode = temp.next;

            if (temp.random != null){
                copyNode.random = temp.random.next; // for reference go to image ...
            }
            else copyNode.random = null;
            temp = temp.next.next;
        }
    }

    private Node getDeepCopyList(Node head){

        Node temp = head;
        Node dummyNode = new Node(-1);
        Node res = dummyNode;

        while (temp != null){

            // creating new list
            res.next = temp.next;
            res = res.next;

            // disconnecting and going back to 
            // initial state o the LL
            temp.next = temp.next.next;
            temp = temp.next;
        }

        return dummyNode.next;
    }   
}
```

#### Design a Browser History Linked List implementation
https://leetcode.com/problems/design-browser-history/description/
```java
class BrowserHistory {

    public class Node{
        String data;
        Node next, back;
        public Node(String url) {
            this.data = url;
            next = null;
            back = null;
        }
    }

    Node currPage;

    public BrowserHistory(String homepage) {
        currPage = new Node(homepage);
    }
    
    public void visit(String url) {
        Node newNode = new Node(url);
        currPage.next = newNode;
        newNode.back = currPage;
        currPage = newNode;
    }
    
    public String back(int steps) {
        
        while (steps != 0){
            if (currPage.back != null){
                currPage = currPage.back;
            }
            else break;
            steps--;
        }
        return currPage.data;
    }
    
    public String forward(int steps) {
        
        while (steps != 0){
            if (currPage.next != null){
                currPage = currPage.next;
            }
            else break;
            steps--;
        }
        return currPage.data;    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * String param_2 = obj.back(steps);
 * String param_3 = obj.forward(steps);
 */
```