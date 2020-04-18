## Array 



### 152. Maximum Product Subarray

- Runtime: **1 ms**, faster than 98.64% of Java online submissions for Maximum Product Subarray.

- Memory Usage: **38.5 MB**, less than 9.76% of Java online submissions for Maximum Product Subarray.

  ⚠️ : How to get **imax**

```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }
        
        int res = nums[0];
        for (int i = 1, imax = res, imin = res; i < nums.length; i++) {
            int temp = imax;
            imax = Math.max(Math.max(imax * nums[i], imin * nums[i]), nums[i]);
            imin = Math.min(Math.min(temp * nums[i], imin * nums[i]), nums[i]);
            
            res = Math.max(res, imax);
        }
        
        return res;
    }
}
```



---

   

  