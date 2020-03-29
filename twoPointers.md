## Two Pointers



### 26. Remove Duplicates from Sorted Array

- Runtime: **0 ms**

- Memory Usage: **41.6 MB**

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        
        int i = 0;
        for (int j = 1; j < nums.length; j++) {
            if (nums[j] != nums[i]){
                i++;
                nums[i] = nums[j];
            }
        }
        
        return i + 1;
    }
}
```



---

### 27. Remove Duplicates from Sorted Array II

- Runtime: **0 ms**

- Memory Usage: **41.5 MB**

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0){
            return 0;
        }
        
        int i = 0;
        int cnt = 1;
        for (int j = 1; j < nums.length; j++) {
            if (nums[i] == nums[j] && cnt == 1) {
                i++;
                nums[i] = nums[j];
                cnt++;
                continue;
            }
            
            if (nums[i] != nums[j]) {
                i++;
                nums[i] = nums[j];
                cnt = 1;
            }
        }
        return i + 1;
    }
}
```



---

### 167. Two Sum II - Input array is sorted

- Runtime: 1 ms, faster than 46.30% of Java online submissions for Two Sum II - Input array is sorted.

- Memory Usage: 43.1 MB, less than 5.22% of Java online submissions for Two Sum II - Input array is sorted.

**one pointer from the start, the other from the end.**

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        if (numbers == null) return null;
        int i = 0;
        int j = numbers.length -1;
        while (i < j) {
            if (numbers[i] + numbers[j] == target) {
                return new int[]{i + 1, j + 1};
            } else if (numbers[i] + numbers[j] >target) {
                j--;
            } else {
                i++;
            }
        }
        
        return null;
    }
}
```



---

### 633. Sum of Square Numbers

- Runtime: 2 ms, faster than 97.51% of Java online submissions for Sum of Square Numbers.

- Memory Usage: 36.1 MB, less than 7.14% of Java online submissions for Sum of Square Numbers.

```java
class Solution {
    public boolean judgeSquareSum(int c) {
        if (c < 0) {
            return false;
        }
        int i = 0;
        int j = (int) Math.sqrt(c); //pruning
        while (i <= j) {
            if (i * i + j * j == c) {
                return true;
            } else if (i * i + j * j > c) {
                j--;
            } else {
                i++;
            }
        }
        
        return false;
    }
}
```



---

### 345.  Reverse Vowels of a String

- Runtime: 3 ms, faster than 81.30% of Java online submissions for Reverse Vowels of a String.

- Memory Usage: 39.9 MB, less than 75.86% of Java online submissions for Reverse Vowels of a String.

**HashSet's `.contain()` method is O(1)**

```java
class Solution {
    private final static HashSet<Character> vowels = new HashSet<>(
        Arrays.asList('a', 'e', 'i', 'o','u', 'A', 'E', 'I', 'O', 'U'));
   
    public String reverseVowels(String s) {
        int i = 0;
        int j = s.length() - 1;
        char[] ans = new char[s.length()];
        while (i <= j) {
            char ci = s.charAt(i);
            char cj = s.charAt(j);
            if (!vowels.contains(ci)) {
                ans[i++] = ci;
            } else if (!vowels.contains(cj)) {
                ans[j--] = cj;
            } else {
                ans[i++] = cj;
                ans[j--] = ci;
            }
        }
        
        return String.valueOf(ans);  //char[] -> String
    }
}
```



---

### 680. Valid Palindrome II

- Runtime: 7 ms, faster than 62.01% of Java online submissions for Valid Palindrome II.

- Memory Usage: 40.5 MB, less than 5.55% of Java online submissions for Valid Palindrome II.

```java
class Solution {
    public boolean validPalindrome(String s) {
        int i = 0;
        int j = s.length() - 1;
        
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)) {
                return isPalindrome(s, i + 1, j) || isPalindrome(s, i, j - 1);
            }
            i++;
            j--;
        }
        
        return true;
    }
    
    private boolean isPalindrome(String s, int i, int j) {
        while (i < j) {
            if (s.charAt(i) != s.charAt(j)){
                return false;
            }
            i++;
            j--;
        }
        
        return true;
    }
}
```



---

### 88. Merge Sorted Array

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Merge Sorted Array.

- Memory Usage: 38.3 MB, less than 5.94% of Java online submissions for Merge Sorted Array.

**the most important thing is searching from end to start**

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int index1 = m - 1;
        int index2 = n - 1;
        int indexMerge = m + n - 1;
        
        while(indexMerge >= 0) {
            if (index1 < 0) {
                nums1[indexMerge--] = nums2[index2--];
            } else if (index2 < 0) {
                nums1[indexMerge--] = nums1[index1--];
            } else if(nums1[index1] > nums2[index2]) {
                nums1[indexMerge--] = nums1[index1--];
            } else {
                nums1[indexMerge--] = nums2[index2--];
            }
        }
    }
}
```



---

### 141. Linked List Cycle

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Linked List Cycle.

- Memory Usage: 40.4 MB, less than 5.71% of Java online submissions for Linked List Cycle.

**If there is a cycle in the linked list, two pointer with different speed will meet.**

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
        if (head == null) {
            return false;
        }
        ListNode l1 = head;
        ListNode l2 = head.next;
        while (l1 != null && l2 != null && l2.next != null) {
            if (l1 == l2) {
                return true;
            }
            l1 = l1.next;
            l2 = l2.next.next;
        }
        
        return false;
    }
}
```



---

### 524. Longest Word in Dictionary through Deleting

- Runtime: 22 ms, faster than 41.45% of Java online submissions for Longest Word in Dictionary through Deleting.

- Memory Usage: 40.8 MB, less than 27.27% of Java online submissions for Longest Word in Dictionary through Deleting.

```java
class Solution {
    private String longestWord = "";
    public String findLongestWord(String s, List<String> d) {
        
        for (String target : d) {
            if (target.length() < longestWord.length()
         ||(target.length() == longestWord.length() && longestWord.compareTo(target) < 0)) {
                continue;
            }
            if (isSubStr(s, target)) {
                longestWord = target;
            }
        } 
        
        return longestWord;
    }
    
    private boolean isSubStr(String s, String target) {
        int i = 0;
        int j = 0;
        
        while (i < s.length() && j < target.length()) {
            if (s.charAt(i) == target.charAt(j)) {
                j++;
            }
            i++;
        }
        
        return j == target.length();
    }
}
```



---

