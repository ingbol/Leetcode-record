## Sort



### 215. Kth Largest Element in an Array

**Solution 1: *Arrays.sort()***

- Runtime: **1 ms**

- Memory Usage: **40.3 MB**
- Time complexity: **O(nlogn)** (quick sort)
- Space complexity: **O(logn)**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
```



Solution 2: **PriorityQueue**

- Runtime: **4 ms**

- Memory Usage: **40 MB**
- TC: **O(NlogK)** ------ add(), poll() is **O(logK)**
- SC: **O(K)**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int val : nums) {
            pq.add(val);
            if (pq.size() > k) {
                pq.poll();
            }
        }
        
        return pq.peek();
    }
}
```



Solution 3: **Quick Select**

- Runtime: **17 ms**

- Memory Usage: **40.9 MB**
- TC: **O(n)**  each time half: n + n/2 + ... + 1
- SC: **O(1)**

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        //quick select
        return findKthSmallest(nums, 0, nums.length - 1, nums.length - k);
    }
    
    private int findKthSmallest(int[] nums, int start, int end, int k) {
        
        int pivot = nums[end];
        int left = start;
        for (int i = start; i < end; i++) {
            if (nums[i] < pivot) {
                swap(nums, left++, i);
            }
        }
        swap(nums, left, end);
        
        if (k == left) {
            return nums[left];
        } else if (k > left) {
            return findKthSmallest(nums, left + 1, end, k);
        } else {
            return findKthSmallest(nums, start, left - 1, k);
        }
    }
    
    private void swap(int[]A, int i, int j) {
        int tmp = A[i];
        A[i] = A[j];
        A[j] = tmp;
    }
}
```



---

### 347. Top K Frequent Elements

- Runtime: **8 ms**, faster than 95.29% of Java online submissions for Top K Frequent Elements.

- Memory Usage: **42.3 MB**, less than 7.76% of Java online submissions for Top K Frequent Elements.

**Bucket Sort**

```java
class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freqs = new HashMap<>();
        for (int num : nums) {
            freqs.put(num, freqs.getOrDefault(num, 0) + 1);
        }
        
        List<Integer>[] buckets = new ArrayList[nums.length + 1];
        for (int key : freqs.keySet()) {
            int frequency = freqs.get(key);
            if (buckets[frequency] == null) {
                buckets[frequency] = new ArrayList<>();
            }
            buckets[frequency].add(key);
        }
        
        List<Integer> topK = new ArrayList<>();
        for (int i = buckets.length - 1; i >= 0 && topK.size() < k; i--) {
            if (buckets[i] == null) {
                continue;
            }
            if (buckets[i].size() <= k - topK.size()) {
                topK.addAll(buckets[i]);
            } else {
                topK.addAll(buckets[i].subList(0,k - topK.size()));
            }
        }
        
        return topK;
    }
}
```

