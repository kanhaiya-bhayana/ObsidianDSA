A **tree data structure** is a hierarchical structure that is used to represent and organize data in a way that is easy to navigate and search. It is a collection of nodes that are connected by edges and has a hierarchical relationship between the nodes. 

The topmost node of the tree is called the root, and the nodes below it are called the child nodes. Each node can have multiple child nodes, and these child nodes can also have their own child nodes, forming a recursive structure.

## Basic Terminologies In Tree Data Structure:

- ****Parent Node:**** The node which is a predecessor of a node is called the parent node of that node. ****{B}**** is the parent node of ****{D, E}****.
- ****Child Node:**** The node which is the immediate successor of a node is called the child node of that node. Examples: ****{D, E}**** are the child nodes of ****{B}.****
- ****Root Node:**** The topmost node of a tree or the node which does not have any parent node is called the root node. {A****}**** is the root node of the tree. A non-empty tree must contain exactly one root node and exactly one path from the root to all other nodes of the tree.
- ****Leaf Node or External Node:**** The nodes which do not have any child nodes are called leaf nodes. ****{K, L, M, N, O, P, G}**** are the leaf nodes of the tree.
- ****Ancestor of a Node:**** Any predecessor nodes on the path of the root to that node are called Ancestors of that node. ****{A,B}**** are the ancestor nodes of the node ****{E}****
- ****Descendant:**** Any successor node on the path from the leaf node to that node. ****{E,I}**** are the descendants of the node ****{B}.****
- ****Sibling:**** Children of the same parent node are called siblings. ****{D,E}**** are called siblings.
- ****Level of a node:**** The count of edges on the path from the root node to that node. The root node has level ****0****.
- ****Internal node:**** A node with at least one child is called Internal Node.
- ****Neighbour of a Node:**** Parent or child nodes of that node are called neighbors of that node.
- ****Subtree****: Any node of the tree along with its descendant.

![[Treedatastructure.png]]


#### Types of BT
1. **Full Binary Tree** 
2. **Complete Binary Tree** 
3. **Perfect Binary Tree** 
4. **Balanced BT** 
5. **Degenerate Tree**

### 1. Full Binary Tree

**Full Binary Tree** is a Binary Tree in which every node has 0 or 2 children.

> **Interesting Fact:** For Full Binary Tree, following equation is always true.
> 
> Number of Leaf nodes = Number of Internal nodes + 1

![[1 fh2By4u-SxTlt6u2xHqnCg.webp]]


# 2. Complete Binary Tree

**Complete Binary Tree** has all levels completely filled with nodes except the last level and in the last level, all the nodes are as left side as possible.

![](https://miro.medium.com/v2/resize:fit:1000/1*M1qfRR59TR9-i4pmI-_Clg.png)

> **Interesting Fact:** Binary Heap is an important use case of Complete Binary tree.

# 3. Perfect Binary Tree

**Perfect Binary Tree** is a Binary Tree in which all internal nodes have 2 children and all the leaf nodes are at the same depth or same level.

![](https://miro.medium.com/v2/resize:fit:1000/1*EgcvwUHXnmdOpbHQwgCknA.png)

>**Interesting Fact:** Total number of nodes in a Perfect Binary Tree with height **H** is **2^H — 1**.

# 4. Balanced Binary Tree

**Balanced Binary Tree** is a Binary tree in which height of the left and the right sub-trees of every node may differ by at most 1.

![](https://miro.medium.com/v2/resize:fit:1000/1*jSq-xjEZYytNDIBpZNQC2w.png)

> **Interesting Fact:** AVL Tree and Red-Black Tree are well-known data structure to generate/maintain Balanced Binary Search Tree. Search, insert and delete operations cost O(log n) time in that.

# 5. Degenerate(or Pathological) Binary Tree

**Degenerate Binary Tree** is a Binary Tree where every parent node has only one child node.

![](https://miro.medium.com/v2/resize:fit:1000/1*m5BjLJeSrSGH4US-QXj4aA.png)

> **Interesting Fact:** Height of a Degenerate Binary Tree is equal to Total number of nodes in that tree.

Degenerate Binary tree is of two types:

- ****Left-skewed Tree:****  If all the nodes in the degenerate tree have only a left child.
- ****Right-skewed Tree:**** If all the nodes in the degenerate tree have only a right child.


#### Node 

```java
class Node{
	int data;
	Node left;
	Node right;
	Node(int key){
		this.data = key
	}
}

public static void main(String []args){
	Node root = new Node(1);
	root.left = new Node(2);
	root.right = new Node(3);
	root.right.left = new Node(4);
}
```


#### Traversal Techniques (BFS/DFS)
	#### DFS - 3 Types
	1. Inorder Traversal (Left, Root, Right)
	2. Pre-order Traversal (Root, Left, Right)
	3. Post-ordr Traversal (Left, Right, Root)

	#### BFS 
	Level order traversal 


#### Traversals
	Recursive

```java
private static void preOrderTraversal(Node root)
{  
  
    // base case   
	if (root == null) return;  
  
    System.out.println(root.data);  
    preOrderTraversal(root.left);  
    preOrderTraversal(root.right);  
}  
  
private static void inOrderTraversal(Node root)
{  
  
    // base case   
	if (root == null) return;  
  
    inOrderTraversal(root.left);  
    System.out.println(root.data);  
    inOrderTraversal(root.right);  
}  
  
private static void postOrderTraversal(Node root)
{  
  
    // base case   
	if (root == null) return;  
  
    inOrderTraversal(root.left);  
    inOrderTraversal(root.right);  
    System.out.println(root.data);  
}
```

	Iterative

```java
private void iterativePreOrderTraversalUtil(TreeNode root){  
  
    // base case  
    if (root == null) return;  
    Stack<TreeNode> st = new Stack<>();  
    st.push(root);  
    while (!st.isEmpty()){  
        root = st.pop();  
        res.add(root.val);  
        if (root.right != null) st.push(root.right);  
        if (root.left != null) st.push(root.left);  
    }  
}
```


#### Pre-order | In-order | Post-order Traversal in one Traversal

```java

class Solution{
	public void preInPostTraversal(TreeNode root){
		Stack<Pair> st = new Stack<>();
		st.push(new Pair(root,1));
		List<Integer> pre = new ArrayList<>();
		List<Integer> in = new ArrayList<>();
		List<Integer> post = new ArrayList<>();
		
		if (root = null) return;
		
		while (!st.isEmpty()){
			Pair it = st.pop();
			
			// this is for pre
			if (it .num ==1){
				pre.add(it.node.val);
				it.num++;
				st.push(it);
				
				if (it.node.left != null){
					st.push(new Pair(it.node.left, 1));
				}
			}
			
			// this is for in
			else if (it.num == 2){
				in.add(it.node.val);
				it.num++;
				st.push(it);
				
				if (it.node.right != null){
					st.push(new Pair(it.node.right, 1));
				}
			}
			
			// don't push it back again
			else{
				post.add(it.node.val);
			}
		}
	}
}
```





# Binary Search Tree
A ****Binary Search Tree**** is a data structure used in computer science for organizing and storing data in a sorted manner. Each node in a ****Binary Search Tree**** has at most two children, a ****left**** child and a ****right**** child, with the ****left**** child containing values less than the parent node and the ****right**** child containing values greater than the parent node. This hierarchical structure allows for efficient ****searching****, ****insertion****, and ****deletion**** operations on the data stored in the tree.

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20221215114732/bst-21.png)


