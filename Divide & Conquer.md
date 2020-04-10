## Divide and Conquer



### 241. Different Ways to Add Parentheses

- Runtime: 1 ms, faster than 97.86% of Java online submissions for Different Ways to Add Parentheses.

- Memory Usage: 40 MB, less than 66.67% of Java online submissions for Different Ways to Add Parentheses.

```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> ways = new ArrayList<>();
        
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (c == '+' || c =='-' || c == '*') {
                List<Integer> left = diffWaysToCompute(input.substring(0, i));
                List<Integer> right = diffWaysToCompute(input.substring(i + 1));
                
                for (int l : left) {
                    for (int r : right) {
                        switch (c) {
                            case '+' :
                                ways.add(l + r);
                                break;
                            case '-' :
                                ways.add(l - r);
                                break;
                            case '*' :
                                ways.add(l * r);
                                break;
                            default:
                                break;
                        }
                    }
                }
            }
        }
        
      	// only contains 1 number
        if (ways.size() == 0) {
            ways.add(Integer.valueOf(input));
        }
        return ways;
    }
}
```



---

### 95. Unique Binary Search Trees II 

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n < 1) {
            return new LinkedList<TreeNode>();
        }
        
        return generateSubTrees(1, n);
    }
    
    private List<TreeNode> generateSubTrees(int s, int e) { // start, end
        List<TreeNode> res = new LinkedList<TreeNode>();
        if (s > e){
            res.add(null);
            return res;
        }
        for (int i = s; i <= e; i++){
            List<TreeNode> left = generateSubTrees(s, i - 1);
            List<TreeNode> right = generateSubTrees(i + 1, e);
            
            for (TreeNode l : left) {
                for (TreeNode r : right) {
                    TreeNode root = new TreeNode(i);
                    root.left = l;
                    root.right = r;
                    res.add(root);
                }
            }
            
        }
        
        return res;
    }
}
```

