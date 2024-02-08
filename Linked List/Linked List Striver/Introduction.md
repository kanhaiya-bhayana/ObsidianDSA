##### Linked List Bootcamp

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
##### Important terms
- Traversal in LL - O(n)
- Length of LL - O(n)
- Search an element in LL - O(n)
- Delete a node in LL - O(n)
- Insert a node in a LL - O(n)
##### Convert an Array to Linked List
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


##### Doubly Linked List
- A **doubly linked list** (DLL) is a special type of linked list in which each node contains a pointer to the previous node as well as the next node of the linked list.

![[DLL1.png]]

##### DLL Node Structure
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


##### Doubly Linked List Implementation
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


##### Reverse a DLL
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
##### Add Two Numbers

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
##### Odd & Even Linked List
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

##### Sort a LinkedList of 0's, 1's and 2's | Multiple Approaches
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
##### Nth node from end of linked list
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
##### Reverse Using Recursion
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
##### LinkedList is Palindrome or not
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
##### Add 1 to a number represented as linked list
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

##### Intersection of Two Linked Lists 
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




##### Find the Middle element of the Linked List

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
##### Detect a loop or cycle in the Linked List
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



##### Find the length of the Loop in LinkedList
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