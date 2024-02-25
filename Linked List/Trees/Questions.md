#### Binary Tree In-order Traversal
1. [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
       public List<Integer> res = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {

        // recursiveInorderTraversalUtil(root);
        iterativeInorderTraversalUtil(root);
        return res;
    }

    private void recursiveInorderTraversalUtil(TreeNode root){

        // base case 
        if (root == null){
            return;
        }
        recursiveInorderTraversalUtil(root.left);
        res.add(root.val);
        recursiveInorderTraversalUtil(root.right);
    }

    private void iterativeInorderTraversalUtil(TreeNode root){

        // base case
        if (root == null) return;

        Stack<TreeNode> st = new Stack<>();

        TreeNode node = root;
        while (true){
            if (node != null){
                st.push(node);
                node = node.left;
            }
            else{
                if (st.isEmpty()){
                    break;
                }
                node = st.pop();
                res.add(node.val);
                node = node.right;
            }
        }
    }
}
```
#### Binary Tree Preorder Traversal 
https://leetcode.com/problems/binary-tree-level-order-traversal/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> wrapList = new ArrayList<>();

        if (root == null) return wrapList;

        queue.offer(root);

        while (!queue.isEmpty()){
            int levelNum = queue.size();
            List<Integer> subList = new ArrayList<>();
            for (int i = 0; i < levelNum; i++){
                if (queue.peek().left != null) queue.offer(queue.peek().left);
                if (queue.peek().right != null) queue.offer(queue.peek().right);
                subList.add(queue.poll().val);
            }
            wrapList.add(subList);
        }

        return wrapList;
    }
}
```

#### Binary Tree Post-order Traversal
https://leetcode.com/problems/binary-tree-postorder-traversal/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<Integer> res;
    public List<Integer> postorderTraversal(TreeNode root) {
        
        res = new ArrayList<>();
        recursivePostorderTraversalUtil(root);
        // iterativePostorderTraversalUtil(root);

        return res;

    }
    
    private void recursivePostorderTraversalUtil(TreeNode root){

        // base case 
        if (root == null) return;

        recursivePostorderTraversalUtil(root.left);
        recursivePostorderTraversalUtil(root.right);
        res.add(root.val);
    }

    private void  iterativePostorderTraversalUtil(TreeNode root){

        // base case
        if (root == null) return;

        Stack<TreeNode> st1 = new Stack<>();
        Stack<TreeNode> st2 = new Stack<>();

        st1.push(root);

        while (!st1.isEmpty()){
            root = st1.pop();
            st2.push(root);
            if (root.left != null) st1.push(root.left);
            if (root.right != null) st1.push(root.right);
        }

        while (!st2.isEmpty()){
            res.add(st2.pop().val);
        }
    }
}
```
#### Binary Tree Level Order Traversal 
https://leetcode.com/problems/binary-tree-level-order-traversal/description/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> wrapList = new ArrayList<>();

        if (root == null) return wrapList;

        queue.offer(root);

        while (!queue.isEmpty()){
            int levelNum = queue.size();
            List<Integer> subList = new ArrayList<>();
            for (int i = 0; i < levelNum; i++){
                if (queue.peek().left != null) queue.offer(queue.peek().left);
                if (queue.peek().right != null) queue.offer(queue.peek().right);
                subList.add(queue.poll().val);
            }
            wrapList.add(subList);
        }

        return wrapList;
    }
}
```
#### Maximum Depth of Binary Tree 
https://leetcode.com/problems/maximum-depth-of-binary-tree/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        
        return maxDepthUtil(root);
    }

    private int maxDepthUtil(TreeNode root){

        // base case
        if (root == null) return 0;

        int leftHeight = maxDepthUtil(root.left);
        int rightHeight = maxDepthUtil(root.right);

        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```
#### Balanced Binary Tree
https://leetcode.com/problems/balanced-binary-tree/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        
        return dfsHeight(root) != -1;
    }

    private int dfsHeight(TreeNode root){

        // base case
        if (root == null) return 0;

        int leftHeight = dfsHeight(root.left);
        if (leftHeight == -1) return -1;

        int rightHeight = dfsHeight(root.right);
        if (rightHeight == -1) return -1;
        
        if (Math.abs(rightHeight - leftHeight) > 1) return -1;

        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```
#### Diameter of a Binary Tree
https://leetcode.com/problems/diameter-of-binary-tree/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int maxi;
    public int diameterOfBinaryTree(TreeNode root) {
        
        maxi = Integer.MIN_VALUE;
        findHeight(root);
        return maxi;
    }

    private int findHeight(TreeNode root){
        if (root == null){
            return 0;
        } 
        int leftHeight = findHeight(root.left);
        int rightHeight = findHeight(root.right);
        maxi = Math.max(maxi,leftHeight + rightHeight);
        return 1 + Math.max(leftHeight,rightHeight);
    }
}
```
#### Maximum Path Sum in Binary Tree

https://leetcode.com/problems/binary-tree-maximum-path-sum/description/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int maxi;
    public int maxPathSum(TreeNode root) {
        maxi = Integer.MIN_VALUE;
        findMaxPathSum(root);
        return maxi;
    }

    private int findMaxPathSum(TreeNode root){
        // base case
        if (root == null){
            return 0;
        }
        int left = Math.max(0,findMaxPathSum(root.left));
        int right = Math.max(0,findMaxPathSum(root.right));
        maxi = Math.max(maxi, left + right + root.val);

        return root.val + Math.max(left, right);
    }
}
```

#### Check if two Trees are identical or Not
	Do any traversal, if you get the same output, then true else false.

https://leetcode.com/problems/same-tree/description/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        return isSameTreeUtil(p,q);
    }

    private boolean isSameTreeUtil(TreeNode p, TreeNode q){
        if (p == null || q == null){
            return (p == q);
        }

        boolean left = isSameTreeUtil(p.left,q.left);
        boolean right = isSameTreeUtil(p.right,q.right);

        return (p.val == q.val) && left && right;

    }
}
```
#### Binary Tree Zigzag Level Order Traversal 
https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/description/

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
        Queue<TreeNode> q = new LinkedList<>();
        List<List<Integer>> wrapList = new ArrayList<>();

        if (root == null) return wrapList;

        q.offer(root);
        boolean leftToRight = true;

        while (!q.isEmpty()){
            int levelNum = q.size();
            List<Integer> subList = new ArrayList<>();
            for (int i = 0; i< levelNum; i++){
                if (q.peek().left != null) q.offer(q.peek().left);
                if (q.peek().right != null) q.offer(q.peek().right);
                
                subList.add(q.poll().val);
            }
            if (!leftToRight){
                Collections.reverse(subList);
            }
            wrapList.add(subList);
            leftToRight = !leftToRight;
        }

        return wrapList;

    }
}
```
#### Boundary Traversal in Binary Tree (Anti-Clock wise)
	1. Add left boundary exclusive leaf nodes
	2. Add leaf nodes
	3. Add right boundary in reverse order exclusive leaf nodes

https://www.geeksforgeeks.org/problems/boundary-traversal-of-binary-tree/1

```java
//User function Template for Java

// class Node  
// { 
//     int data; 
//     Node left, right; 
   
//     public Node(int d)  
//     { 
//         data = d; 
//         left = right = null; 
//     } 
// }

class Solution
{
	ArrayList <Integer> boundary(Node root)
	{
	    ArrayList<Integer> res = new ArrayList<>();
	    
	    if (root == null) return res;
	    
	    if (isLeaf(root) == false) res.add(root.data);
	    
	    addLeftBoundaryUtil(root,res);
	    addLeavesUtil(root,res);
	    addRightBoundaryUtil(root,res);
	    
	    return res;
	}
	
	private void addLeftBoundaryUtil(Node root, ArrayList<Integer> res){
	    Node curr = root.left;
	    while (curr != null){
	        if (isLeaf(curr) == false) res.add(curr.data);
	        if (curr.left != null) curr = curr.left;
	        else curr = curr.right;
	    }
	}
	
	private void addLeavesUtil(Node root, ArrayList<Integer> res){
	    if (isLeaf(root)){
	        res.add(root.data);
	        return;
	    }
	    if (root.left != null) addLeavesUtil(root.left, res);
	    if (root.right != null) addLeavesUtil(root.right, res);
	}
	
	private void addRightBoundaryUtil(Node root, ArrayList<Integer> res){
	    Node curr = root.right;
	    ArrayList<Integer> tmp = new ArrayList<>();
	    while (curr != null){
	        if (isLeaf(curr) == false) tmp.add(curr.data);
	        if (curr.right != null) curr = curr.right;
	        else curr = curr.left;
	    }
	    for (int i = tmp.size()-1; i >=0; i--){
	        res.add(tmp.get(i));
	    }
	}
	
	private boolean isLeaf(Node root){
	    if (root.left == null && root.right == null) return true;
	    else return false;
	}
}
```
#### Vertical Order Traversal of Binary Tree
	- If 2 nodes are overlaping then write in the sort order
- https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/description/
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

 class Tuple{
     TreeNode node;
     int row;
     int col;
     public Tuple(TreeNode _node, int _row, int _col){
         node = _node;
         row = _row;
         col = _col;
     }
 }
class Solution {
    public List<List<Integer>> verticalTraversal(TreeNode root) {
        
        TreeMap<Integer,TreeMap<Integer,PriorityQueue<Integer>>> map = new TreeMap<>();
        Queue<Tuple> q = new LinkedList<>();
        q.offer(new Tuple(root, 0, 0));

        while (!q.isEmpty()){
            Tuple tuple = q.poll();
            TreeNode node = tuple.node;
            int x = tuple.row;
            int y = tuple.col;

            if (!map.containsKey(x)){
                map.put(x, new TreeMap<>());
            }

            if (!map.get(x).containsKey(y)){
                map.get(x).put(y, new PriorityQueue<>());
            }

            map.get(x).get(y).offer(node.val);

            if (node.left != null){
                q.offer(new Tuple(node.left, x-1, y+1));
            }
            if (node.right != null){
                q.offer(new Tuple(node.right, x+1, y+1));
            }
        }

        List<List<Integer>> list = new ArrayList<>();
        for (TreeMap<Integer, PriorityQueue<Integer>> ys : map.values()){
            list.add(new ArrayList<>());
            for (PriorityQueue<Integer> nodes : ys.values()){
                while (!nodes.isEmpty()){
                    list.get(list.size()-1).add(nodes.poll());
                }
            }
        }

        return list;
    }
}
```

#### Top View of Binary Tree

https://www.geeksforgeeks.org/problems/top-view-of-binary-tree/1

```java

/*
class Node{
    int data;
    Node left;
    Node right;
    Node(int data){
        this.data = data;
        left=null;
        right=null;
    }
}
*/
class Pair{
    Node node;
    int hd;
    public Pair(Node _node, int _hd){
        node = _node;
        hd = _hd;
    }
}

class Solution
{
    //Function to return a list of nodes visible from the top view 
    //from left to right in Binary Tree.
    static ArrayList<Integer> topView(Node root)
    {
        // add your code
        ArrayList<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        
        Map<Integer, Integer> map = new TreeMap<>();
        Queue<Pair> q =new LinkedList<Pair>();
        q.add(new Pair(root,0));
        
        while (!q.isEmpty()){
            Pair it = q.remove();
            int hd = it.hd;
            Node temp = it.node;
            
            if (map.get(hd)==null) map.put(hd,temp.data);
            if (temp.left != null){
                q.add(new Pair(temp.left,hd-1));
            }
            
            if (temp.right != null){
                q.add(new Pair(temp.right, hd+1));
            }
        } 
        
        for (Map.Entry<Integer,Integer> entry : map.entrySet()){
            ans.add(entry.getValue());
        }
        return ans;
    }
}
```