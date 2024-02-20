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