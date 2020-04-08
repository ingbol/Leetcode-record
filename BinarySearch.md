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

