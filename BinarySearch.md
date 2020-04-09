## Binary Search

Every loop halves the search region, so the time complexity is O(logN).

compute the middle value:

- m = (l + h) / 2
- m = l + (h - l) / 2

The 1st one  l + h might overflow, but h - l would not,  so the 2nd one is a better choice.



---

### 69. Sqrt(x)

- Runtime: 2 ms, faster than 16.55% of Java online submissions for Sqrt(x).

- Memory Usage: 38.6 MB, less than 5.00% of Java online submissions for Sqrt(x).

**pay attention to `int` overflow**

```java
class Solution {
    public int mySqrt(int x) {
        // binary search
        int left = 1;
        int right = x;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            // using mid * mid < x will overflow and cause problem
            if (x / mid == mid) {
                return mid;
            }
            if (x / mid > mid) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        //return the smaller one
        return right;
    }
}
```



---

### 744. Find Smallest Letter Greater Than Target

- Runtime: 1 ms, faster than 8.47% of Java online submissions for Find Smallest Letter Greater Than Target.

- Memory Usage: 46 MB, less than 6.90% of Java online submissions for Find Smallest Letter Greater Than Target.

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int low = 0;
        int high = letters.length - 1;
        
        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            if (letters[mid] > target) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        
      	// return the larger one or the first element
        return low < letters.length ? letters[low] : letters[0];
    }
}
```



---

### 540. Single Element in a Sorted Array

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Single Element in a Sorted Array.

- Memory Usage: 40.4 MB, less than 8.00% of Java online submissions for Single Element in a Sorted Array.

1. target has to be **even**

2. If mid is even && mid + 1 < target : nums[m] = nums [m + 1] ----> target locates in **[mid + 2, h]**

​															   nums[m] != nums[m + 1] -----> target locates in **[l, mid]**

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid = low + (high - low) / 2;
            
            if (mid % 2 == 1) {
                mid--;
            }
            
            if (nums[mid] == nums[mid + 1]) {
                low = mid + 2;
            } else {
                high = mid;
            }
        }
        
        return nums[low];
    }
}
```



---

### 278. First Bad Version

Solution 1: 

- Runtime: 26 ms, faster than 6.76% of Java online submissions for First Bad Version.

- Memory Usage: 36.2 MB, less than 5.63% of Java online submissions for First Bad Version.

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        //Solution 1
        
         int l = 0;
         int h = n;
      
         while (l <= h) {
             int m = l + (h - l) / 2;
             if (isBadVersion(m) && !isBadVersion(m - 1)) {
                 return m;
             }
             if (isBadVersion(m)) { // m 落在了bad中
                 h = m - 1;
             } else {
                 l = m + 1;
             }
         }
        
         return l;
    }
}
```



Solution 2: **easier to understand, and much quicker**

- Runtime: 12 ms, faster than 98.24% of Java online submissions for First Bad Version.

- Memory Usage: 36.1 MB, less than 5.63% of Java online submissions for First Bad Version.

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        //Solution 2
        int l = 0;
        int h = n;
        
        while (l < h) {
            int m = l + (h - l) / 2;
            if (isBadVersion(m)) {
                h = m;      // target in [l, m]
            } else {
                l = m + 1; // target in [m + 1, h]
            }
        }
        
        return l;
    }
}
```



---

### ⚠️153. Find Minimum in Rotated Sorted Array

- : 0 ms, faster than 100.00% of Java online submissions for Find Minimum in Rotated Sorted Array.

- Memory Usage: 39.2 MB, less than 59.09% of Java online submissions for Find Minimum in Rotated Sorted Array.

**the most important place of this problem is the `if` 's condition**

```java
class Solution {
    public int findMin(int[] nums) {
        int l = 0;
        int h = nums.length - 1;
        
        while (l < h) {
            int m = l + (h - l) / 2;
            if (nums[m] <= nums[h]) { //[l, m] 这个判断条件十分重要
                h = m;
            } else { //[m+1, h]
                l = m + 1;
            }
        }
        
        return nums[l];
    }
}
```

