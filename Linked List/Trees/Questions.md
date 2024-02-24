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