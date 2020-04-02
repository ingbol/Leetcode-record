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



---

### 451. Sort Characters By Frequency

- Runtime: **9 ms**, faster than 91.40% of Java online submissions for Sort Characters By Frequency.

- Memory Usage: **40.1 MB**, less than 37.04% of Java online submissions for Sort Characters By Frequency.

**Bucket Sort**

```java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> freqs = new HashMap<>();
        for(char ch : s.toCharArray()) {
            freqs.put(ch, freqs.getOrDefault(ch, 0) + 1);
        }
        
        List<Character>[] buckets = new ArrayList[s.length() + 1];
        for (char ch : freqs.keySet()) {
            if (buckets[freqs.get(ch)] == null) {
                buckets[freqs.get(ch)] = new ArrayList<>();
            }
            buckets[freqs.get(ch)].add(ch);
        }
        
        StringBuilder str = new StringBuilder();
        for (int i = buckets.length - 1; i >= 0; i--) {
            if (buckets[i] == null) {
                continue;
            } else {
                for(char ch : buckets[i]){
                    for (int j = 0; j < i; j++) {
                        str.append(ch);
                    }
                }
            }
        }
        
        return str.toString();
    }
}
```



---

### 75. Sort Colors

**2 Pass**

- Runtime: **0 ms**

- Memory Usage: **38.3 MB**

```java
class Solution {
    public void sortColors(int[] nums) {
        int[] count = {0, 0, 0};
        for (int num : nums){
            if (num == 0) {
                count[0]++;
            } else if (num == 1) {
                count[1]++;
            } else {
                count[2]++;
            }
        }
        
        int l = 0;
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < count[i]; j++) {
                nums[l] = i;
                l++;
            }
        }
    }
}
```



**1 Pass**

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Sort Colors.

- Memory Usage: 38 MB, less than 5.51% of Java online submissions for Sort Colors.

```java
class Solution {
    public void sortColors(int[] nums) {
        // 1 pass
        
        int zero = 0;
        int one = 0;
        int two = nums.length - 1;
        
        while (one <= two) {
            if (nums[one] == 0) {
                swap(nums, zero++, one++);
            } else if (nums[one] == 2) {
                swap(nums, two--, one);
            } else {
                one++;
            }
        }
    }
    
    private void swap(int[] nums, int a, int b) {
        int temp = nums[a];
        nums[a] = nums[b];
        nums[b] = temp;
    }
}
```



---

