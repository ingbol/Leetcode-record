## Tree



### 144. Binary Tree Preorder Traversal

**Recursive Solution with list**

- Runtime: **0 ms**, faster than 100.00% of Java online submissions for Binary Tree Preorder Traversal.

- Memory Usage: **38.2 MB**, less than 5.17% of Java online submissions for Binary Tree Preorder Traversal.

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        //recursive method with list
        List<Integer> pre = new LinkedList<Integer>();
        if (root == null) {
            return pre;
        }
        pre.add(root.val);
        pre.addAll(preorderTraversal(root.left));
        pre.addAll(preorderTraversal(root.right));
        
        return pre;
    }
    
}
```

**Iterative Solution**

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        // iterate with Stack
        List<Integer> pre = new LinkedList<Integer>();
        if (root == null) {
            return pre;
        }
    
        Stack<TreeNode> tovisit = new Stack<TreeNode>();
        tovisit.push(root);
        
        while (!tovisit.empty()) {
            TreeNode visiting = tovisit.pop();
            pre.add(visiting.val);
           
            if (visiting.right != null) {
                tovisit.push(visiting.right);
            }
            if (visiting.left != null) {
                tovisit.push(visiting.left);
            }
        }
        
        return pre;
    }
}
```

