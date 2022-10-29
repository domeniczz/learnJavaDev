# Binary Tree

## Tree Traversals

Trees can be traversed in different ways: Preorder, Inorder, Postorder

<img src="https://domenic-gallery.oss-cn-hangzhou.aliyuncs.com/DataStructure/Preorder-Inorder-Postorder-traversals.png" width="480rme" style="border-radius:.4rem" float="left" alt="Preorder-Inorder-Postorder-traversals"/><div style="clear:both"></div>

- **In-order traversal:**

  4, 10, 12, 15, 18, 22, 24, 25, 31, 35, 44, 50, 66, 70, 90

- **Pre-order traversal:**

  25, 15, 10, 4, 12, 22, 18, 24, 50, 35, 31, 44, 70, 66, 90

- **Post-order traversal:**

  4, 12, 10, 18, 24, 22, 15, 31, 44, 35, 66, 90, 70, 50, 25

**Use example：**

Node definition:

```java
class Node {
    int key;
    Node left, right;

    public Node(int item) {
        key = item;
        left = right = null;
    }
}
```

Driver code:

```java
public static void main(String[] args) {
    // Init data
    BinaryTree tree = new BinaryTree();
    tree.root = new Node(1);
    tree.root.left = new Node(2);
    tree.root.right = new Node(3);
    tree.root.left.left = new Node(4);
    tree.root.left.right = new Node(5);

    System.out.println("\n traversal of binary tree is: ");
    // Traversal function call
    tree.printPreorder();
}
```

### Inorder

In the case of binary search trees (BST), Inorder traversal gives nodes in **non-decreasing order**.

> When you do the inorder traversal of a binary tree, the neighbors of given node are called: 
>
> - **Predecessor** (the node lies ahead of given node)
> - **Successor** (the node lies behins of given node)

#### Recursive

Java：

```java
// Root of Binary Tree
Node root;

BinaryTree() {
    root = null;
}

/* Given a binary tree, print its nodes in inorder */
void printInorder(Node node) {
    if (null == node) return;

    // first recur on left child
    printInorder(node.left);

    // then deal with the node
    System.out.print(node.key + " ");

    // now recur on right child
    printInorder(node.right);
}

// Wrappers over above recursive functions
void printInorder() {
    printInorder(root);
}
```

**Time Complexity:** O(N)

**Space complexity:** The worst case space required is O(n), and in the average case it's O(log n) where n is the number of nodes.

**Auxiliary Space:** If we don’t consider the size of the stack for function calls then O(1) otherwise O(h) where h is the height of the tree. 

#### Morris Traversal

In this method, we have to use a new data structure - Threaded Binary Tree

A binary tree is made threaded by: making all right child pointers that would normally be NULL point to the inorder successor of the node (if it exists).

**For example:**

```
          1
        /   \
       2     3
      / \   /
     4   5 6
```

First, 1 is the root, so initialize 1 as current, 1 has left child which is 2

The current's left subtree is:

```
         2
        / \
       4   5
```

So in this subtree, the rightmost node is 5, then make the current(1) as the right child of 5. Set current = cuurent.left (current = 2).

The tree now looks like:

```
         2
        / \
       4   5
            \
             1
              \
               3
              /
             6
```

For current 2, which has left child 4, we can continue with thesame process as we did above

```
        4
         \
          2
           \
            5
             \
              1
               \
                3
               /
              6
```

Then add 4 because it has no left child, then add 2, 5, 1, 3 one by one, for node 3 which has left child 6, do the same as above. 

Finally, the inorder taversal is [4, 2, 5, 1, 6, 3]

**Example code:**

Java:

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        TreeNode curr = root;
        TreeNode pre;
        while (curr != null) {
            if (curr.left != null) { // has a left subtree
                pre = curr.left;
                while (pre.right != null) { // find rightmost
                    pre = pre.right;
                }
                pre.right = curr; // put curr after the pre node
                
                TreeNode temp = curr; // store curr node temporarily
                curr = curr.left; // go to next node
                temp.left = null; // original curr left be null, avoid infinite loops
            } else {
                res.add(curr.val);
                curr = curr.right; // move to next right node
            }
        }
        return res;
    }
}
```

### Preorder

Preorder traversal is used to **create a copy** of the tree.

Preorder traversal is also used to get prefix expressions on an expression tree.

Java:

```java
// Root of Binary Tree
Node root;

BinaryTree() {
    root = null;
}

/* Given a binary tree, print its nodes in preorder */
void printPreorder(Node node) {
    if (null == node)
        return;

    // first deal with the node
    System.out.print(node.key + " ");

    // then recur on left subtree
    printPreorder(node.left);

    // now recur on right subtree
    printPreorder(node.right);
}

// Wrappers over above recursive functions
void printPreorder() {
    printPreorder(root);
}
```

**Time Complexity:** O(N)

**Space complexity:** The worst case space required is O(n), and in the average case it's O(log n) where n is the number of nodes.

**Auxiliary Space:** If we don’t consider the size of the stack for function calls then O(1) otherwise O(h) where h is the height of the tree. 

### Postorder

Postorder traversal is used to delete the tree, because before deleting the parent node, we should delete its child nodes first.

Postorder traversal is also useful to get the postfix expression of an expression tree.

Java:

```java
// Root of Binary Tree
Node root;

BinaryTree() {
    root = null;
}

/* Given a binary tree, print its nodes according to the "bottom-up" postorder traversal. */
void printPostorder(Node node) {
    if (null == node)
        return;

    // first recur on left subtree
    printPostorder(node.left);

    // then recur on right subtree
    printPostorder(node.right);

    // now deal with the node
    System.out.print(node.key + " ");
}

// Wrappers over above recursive functions
void printPostorder() {
    printPostorder(root);
}
```

**Time Complexity:** O(N)

**Space complexity:** The worst case space required is O(n), and in the average case it's O(log n) where n is the number of nodes.

**Auxiliary Space:** If we don’t consider the size of the stack for function calls then O(1) otherwise O(h) where h is the height of the tree. 