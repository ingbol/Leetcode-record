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

