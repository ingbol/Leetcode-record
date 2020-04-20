## 字节面试准备

### 1. Two Sum

- Runtime: 2 ms, faster than 64.43% of Java online submissions for Two Sum.

- Memory Usage: 39.5 MB, less than 5.65% of Java online submissions for Two Sum.

  ```java
  class Solution {
      public int[] twoSum(int[] nums, int target) {
          Map<Integer, Integer> map = new HashMap<>();
          for (int i = 0; i < nums.length; i++) {
              map.put(nums[i], i);
          }
          
          for (int i = 0; i < nums.length; i++) {
              int diff = target - nums[i];
              if (map.containsKey(diff) && map.get(diff) != i) {
                  return new int[]{i, map.get(diff)};
              }
          }
          
          return new int[2];
      }
  }
  ```

  

---

### 2. Add Two Numbers

- Runtime: 1 ms, faster than 100.00% of Java online submissions for Add Two Numbers.

- Memory Usage: 39.8 MB, less than 99.37% of Java online submissions for Add Two Numbers.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p = l1;
        ListNode q = l2;
        ListNode res = new ListNode(0);
        ListNode cur = res;
        int carry = 0;
        
        while(p != null || q != null) {
            int x = p != null? p.val: 0;
            int y = q != null? q.val: 0;
            int sum = x + y + carry;
            carry = sum / 10;
            cur.next = new ListNode(sum % 10);
          
            cur = cur.next;
            if (p != null) {
                p = p.next;
            }
            if (q != null) {
                q = q.next;
            }
        }
        
        if (carry > 0) {
            cur.next = new ListNode(carry);
        }
            
        return res.next;
    }
}
```



---

### 3. Longest Substring Without Repeating Characters

- Runtime: 5 ms, faster than 86.50% of Java online submissions for Longest Substring Without Repeating Characters.

- Memory Usage: 39.6 MB, less than 14.16% of Java online submissions for Longest Substring Without Repeating Characters.

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        
        Map<Character, Integer> map = new HashMap<>();
        int n = s.length();
        int res = 0;
        
        for (int i = 0, j = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))){
                i = Math.max(map.get(s.charAt(j)) + 1, i); 
              	//因为map中并没有remove之前的k-v，所以可能会取出小于i的值
            }
            res = Math.max(res, j - i + 1);
            map.put(s.charAt(j), j);
        }
        
        return res;
    }
}
```



---

### 11. Container With Most Water

- Runtime: 6 ms, faster than 28.04% of Java online submissions for Container With Most Water.

- Memory Usage: 46.3 MB, less than 5.77% of Java online submissions for Container With Most Water.

```java
class Solution {
    public int maxArea(int[] height) {
        //使用双指针方法
        //移动长边不会使体积变大，故每次让短边移动
        int max = 0;
        int i = 0;
        int j = height.length - 1;
        
        while (i < j) {
            max = Math.max(max, (j - i) * Math.min(height[i], height[j]));
            if (height[i] < height[j]) {
                i++;
            } else {
                j--;
            }             
        }
        
        return max;
    }
}
```



---

### 21. Merge Two Sorted Lists

- Runtime: 1 ms, faster than 11.15% of Java online submissions for Merge Two Sorted Lists.

- Memory Usage: 40.1 MB, less than 14.48% of Java online submissions for Merge Two Sorted Lists.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode p = l1;
        ListNode q = l2;
        ListNode res = new ListNode(0);
        ListNode cur = res;
        
        while (p != null || q != null) {
            if (p == null) {
                cur.next = q;
                break;
            } else if (q == null) {
                cur.next = p;
                break;
            } else if (p.val < q.val) {
                cur.next = new ListNode(p.val);
                p = p.next;
            } else {
                cur.next = new ListNode(q.val);
                q = q.next;
            }
            
            cur = cur.next;
        }
        
        return res.next;
    }
}
```



---

### 23. Merge k Sorted Lists

- Runtime: 7 ms, faster than 26.34% of Java online submissions for Merge k Sorted Lists.

- Memory Usage: 41.3 MB, less than 42.63% of Java online submissions for Merge k Sorted Lists.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        List<Integer> nodes = new ArrayList<>();
        
        for (ListNode ln : lists) {
            while (ln != null) {
                nodes.add(ln.val);
                ln = ln.next;
            }
        }
        
        nodes.sort((o1, o2) -> o1 - o2);
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        
        for (int node : nodes) {
            cur.next = new ListNode(node);
            cur = cur.next;
        }
        
        return dummy.next;
    }
}
```



---

### 25. Reverse Nodes in k-Group

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Reverse Nodes in k-Group.

- Memory Usage: 43.1 MB, less than 5.17% of Java online submissions for Reverse Nodes in k-Group.

链表的reverse，十分有难度。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null || k == 1) {
            return head;
        }
        
        ListNode dummyhead = new ListNode(0);
        dummyhead.next = head;
        ListNode begin = dummyhead;
        int i = 0;
        
        while (head != null) {
            i++;
            if (i % k == 0) {
                begin = reverse(begin, head.next);
                head = begin.next;
            } else {
                head = head.next;
            }
        }
        
        return dummyhead.next;
    }
    
    public ListNode reverse(ListNode begin, ListNode end) {
        ListNode curr = begin.next;
        ListNode prev = begin;
        ListNode first = curr;
        ListNode next;
        
        while (curr != end) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        begin.next = prev;
        first.next = curr;
        
        return first;
    }
}
```



---

### 33. Search in Rotated Sorted Array

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Search in Rotated Sorted Array.

- Memory Usage: 39.3 MB, less than 20.13% of Java online submissions for Search in Rotated Sorted Array.

此题应充分理解判断条件。

```java
class Solution {
    public int search(int[] nums, int target) {
        int l = 0;
        int h = nums.length - 1;
        
        while (l <= h) {
            int m = l + (h - l) / 2;
            
            if (nums[m] == target) {
                return m;
            }
            
            if (nums[m] >= nums[l]) { //通过分析得到，=等号必须取到
                if (target >= nums[l] && target < nums[m]){
                    h = m - 1;
                } else {
                    l = m + 1;
                }
            } else {
                if (target > nums[m] && target <= nums[h]) {
                    l = m + 1;
                } else {
                    h = m - 1;
                }
            }
        }
        
        return -1;
    }
}
```



---

### 41. First Missing Positive

- Runtime: 0 ms, faster than 100.00% of Java online submissions for First Missing Positive.

- Memory Usage: 37.6 MB, less than 6.85% of Java online submissions for First Missing Positive.

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while(nums[i] > 0 && nums[i] < n && nums[i] != nums[nums[i] - 1]){
                //nums[i]的下标应该为 nums[i] - 1
                swap(nums, i, nums[i] - 1);
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
               return i + 1;
            }
        }
        
        //nums中n项都满足nums[i] = i + 1
        return n + 1;
    }
    
    public void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```



---

### 42. Trapping Rain Water

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Trapping Rain Water.

- Memory Usage: 39.4 MB, less than 26.71% of Java online submissions for Trapping Rain Water.

```java
class Solution {
    public int trap(int[] height) {
        //用动态规划，储存第i列对应的 max_left[i] 和 max_right[i]
        int[] maxLeft = new int[height.length];
        int[] maxRight = new int[height.length];
        int ans = 0;
        
        //计算maxLeft[]
        for (int i = 1; i < height.length - 1; i++) {
            maxLeft[i] = Math.max(maxLeft[i - 1], height[i - 1]);
        }
        
        //计算maxRight[]
        for (int i = height.length - 2; i >= 0; i--) {
            maxRight[i] = Math.max(maxRight[i + 1], height[i + 1]);
        }
        
        //计算每一列储水量
        for (int i = 0; i < height.length - 1; i++) {
            int h = Math.min(maxRight[i], maxLeft[i]);
            ans += Math.max(h - height[i], 0);
        }
        
        return ans;
        
    }
}
```



---

### 54. Spiral Matrix

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Spiral Matrix.

- Memory Usage: 36.7 MB, less than 5.21% of Java online submissions for Spiral Matrix.

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if (matrix.length == 0) {
            return new ArrayList<Integer>();
        }
        
        List<Integer> ans = new ArrayList<>();
        int direction = 0; //0:Right 1:Down 2:Left 3:Up
        
        int rightBorder = matrix[0].length - 1;
        int downBorder = matrix.length - 1;
        int leftBorder = 0;
        int upBorder = 0;
        int currX = 0;
        int currY = 0;
        
        while (true) {
            
            if (ans.size() == matrix.length * matrix[0].length) {
                break;
            }
            
            ans.add(matrix[currY][currX]);
            
            switch (direction) {
                //当前向右
                case 0:
                    if(currX == rightBorder) {
                        direction = 1;
                        upBorder++;
                        currY++;
                    } else {
                        currX++;
                    }
                    break;
                
                //向下
                case 1:
                    if (currY == downBorder) {
                        direction = 2;
                        rightBorder--;
                        currX--;
                    } else {
                        currY++;
                    }
                    break;
                   
                //向左
                case 2:
                    if (currX == leftBorder) {
                        direction = 3;
                        downBorder--;
                        currY--;
                    } else {
                        currX--;
                    }
                    break;
                    
                //向上
                case 3:
                    if (currY == upBorder) {
                        direction = 0;
                        leftBorder++;
                        currX++;
                    } else {
                        currY--;
                    }
                    break;
            }
        }
        return ans;
    }
}
```



---

### 103. Binary Tree Zigzag Level Order Traversal

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Binary Tree Zigzag Level Order Traversal.

- Memory Usage: 38.1 MB, less than 5.77% of Java online submissions for Binary Tree Zigzag Level Order Traversal.

分层遍历二叉树

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(root, 0, ans);
        return ans;
    }
    
    public void dfs(TreeNode root, int level, List<List<Integer>> ans) {
        if (root == null) {
            return;
        }
        
        if (ans.size() <= level) {
            ans.add(new ArrayList<Integer>());
        }
        
        if (level % 2 == 0) {
            ans.get(level).add(root.val);
        } else {
            ans.get(level).add(0, root.val);
        }
        
        dfs(root.left, level + 1, ans);
        dfs(root.right, level + 1, ans);
    }
}
```

