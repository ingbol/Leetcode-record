## Greedy



### 649. Dota2 Senate

- Runtime: **7 ms**

- Memory Usage: **41.8 MB**

  ⚠️ ：use Queue

```java
class Solution {
    public String predictPartyVictory(String senate) {
        Queue<Integer> queue = new LinkedList();
        int[] people = new int[]{0, 0};
        int[] bans = new int[]{0, 0};
        
        for (char person: senate.toCharArray()) {
            int x = person == 'R' ? 1 : 0;
            people[x]++;
            queue.add(x);
        }
        
        while(people[0] > 0 && people[1] > 0){
            int x = queue.poll();
            if(bans[x] > 0){
                bans[x]--;
                people[x]--;
            } else {
                bans[x^1]++;
                queue.add(x);
            }
        }
        
        return people[0] > 0 ? "Dire" : "Radiant";
    }
}
```



---

### 455. Assign Cookies

- Runtime: 6 ms, faster than 99.93% of Java online submissions for Assign Cookies.

- Memory Usage: 40.8 MB, less than 61.90% of Java online submissions for Assign Cookies.

**assign the smallest cookies to the child who is the easiest one to be content.**

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);

        int content = 0;
        int i = 0;
        int j = 0;
        while (i < g.length && j < s.length) {
            if (g[i] <= s[j]) {
                i++;
                content++;
            }
            j++;
        }
        return content;
    }
}
```



---

### 435. Non-overlapping Intervals 

- Runtime: 3 ms, faster than 61.09% of Java online submissions for Non-overlapping Intervals.

- Memory Usage: 39.6 MB, less than 6.25% of Java online submissions for Non-overlapping Intervals.

Greedy Strategy : try to make `endtime` small

use **lambda** will slow down a little bit

```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        // numbers in intervals could be negative number
        Arrays.sort(intervals, (o1, o2) -> o1[1] -o2[1]);
        
        int endtime = Integer.MIN_VALUE;
        int count = 0;
        for (int i = 0; i < intervals.length; i++) {
            if (intervals[i][0] >= endtime) {
                count++;
                endtime = intervals[i][1];
            }
        }
        
        return intervals.length - count;
            
    }
}
```



---

### 452. Minimum Number of Arrows to Burst Balloons

- Runtime: **14 ms**, faster than 99.87% of Java online submissions for Minimum Number of Arrows to Burst Balloons.

- Memory Usage: 47.2 MB, less than 14.29% of Java online submissions for Minimum Number of Arrows to Burst Balloons.

```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0) {
            return 0;
        }
        
        Arrays.sort(points, (o1, o2) -> o1[1] -o2[1]);
        
        int curEnd = points[0][1];
        int count = 1;
        for (int i = 1; i < points.length; i++) {
            if (curEnd < points[i][0]) {
                curEnd = points[i][1];
                count++;
            }
        }
        return count;
    }
}
```



---

### 406. Queue Reconstruction by Height

- Runtime: 5 ms, faster than 99.01% of Java online submissions for Queue Reconstruction by Height.

- Memory Usage: 40.6 MB, less than 97.87% of Java online submissions for Queue Reconstruction by Height.

put higher people first , then use `Arrays.add(index, T) ` to insert lower people won't affect other people in the queue.

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        if (people == null || people.length == 0) {
            return new int[0][0];
        }
        
        Arrays.sort(people, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0]);
        
        List<int[]> res = new ArrayList<>();
        for (int[] p : people) {
            res.add(p[1], p);
        }
        
        return res.toArray(new int[res.size()][2]);
    }
}
```



---

### 121. Best Time to Buy and Sell Stock

- Runtime: 1 ms, faster than 99.09% of Java online submissions for Best Time to Buy and Sell Stock.

- Memory Usage: 39.7 MB, less than 14.16% of Java online submissions for Best Time to Buy and Sell Stock.

```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0){
            return 0;
        }
        
        int min = prices[0];
        int profit = 0;
        
        for(int i = 1; i < prices.length; i++){
            if( prices[i] < min ){
                min = prices[i];
            } else if( profit < prices[i] - min ){
                profit = prices[i] - min;
            }
        }
        
        return profit;
    }
}
```



---

### 122. Best Time to Buy and Sell Stock II

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Best Time to Buy and Sell Stock II.

- Memory Usage: 40.1 MB, less than 9.52% of Java online submissions for Best Time to Buy and Sell Stock II.

get all profit

[a, b, c, d], if `a <= b <= c <= d` , the `maxProfit = d - a =(d - c) + (c - b) + (b - a)` 

```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        
        for (int i = 1; i < prices.length; i++) {
            if(prices[i] > prices[i-1]) {
                profit += prices[i] - prices[i-1];
            }
        }
        
        return profit;
        
    }
}
```



---

### 605. Can Place Flowers

- Runtime: 1 ms, faster than 96.09% of Java online submissions for Can Place Flowers.

- Memory Usage: 41 MB, less than 10.71% of Java online submissions for Can Place Flowers.

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int count = 0;
        
        for (int i = 0; i < flowerbed.length; i++) {
            if (flowerbed[i] == 1) {
                continue;
            }
            
          	//include boundary conditions
            int pre = i == 0 ? 0 : i - 1;
            int next = i == flowerbed.length - 1? flowerbed.length - 1 : i + 1;
            if (flowerbed[pre] == 0 && flowerbed[next] == 0) {
                count++;
                flowerbed[i] = 1;
            }
        }
        
        return count >= n;
    }
}
```



---

### 392. Is Subsequence

- Runtime: 1 ms, faster than 100.00% of Java online submissions for Is Subsequence.

- Memory Usage: 42.7 MB, less than 100.00% of Java online submissions for Is Subsequence.

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int index = -1;
        
        for(char c : s.toCharArray()) {
          	//.indexOf() will return -1 if it cannot find c
            index = t.indexOf(c, index + 1);
            if (index == -1) {
                return false;
            }
        }
        
        return true;
    }
}
```



---

### 665. Non-decreasing Array

- Runtime: 1 ms, faster than 81.08% of Java online submissions for Non-decreasing Array.

- Memory Usage: 41.1 MB, less than 47.62% of Java online submissions for Non-decreasing Array.

**Do not affect subsequent operations**:  change the value of nums[i] to that of nums[i+1], only if this can not satisfy the requirements, we do reversely.

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        int count = 0;
        
        for (int i = 0; i < nums.length - 1; i++) {           
            if(nums[i] > nums[i+1]) {
                if(i == 0 || nums[i-1] <= nums[i+1]){
                    nums[i] = nums[i+1];
                } else {
                    nums[i+1] =nums[i];
                }
                count++;
            }
            
            if (count > 1) {
                break;
            }
        }
        
        return count <= 1;
    }
}
```



---

#### 53. Maximum Subarray

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Maximum Subarray.

- Memory Usage: 39.4 MB, less than 9.39% of Java online submissions for Maximum Subarray.

**it's fantastic**: when sum < 0, we make sum = nums[i]

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int sum = 0;
        int max = Integer.MIN_VALUE; // consider negative numbers
        
        for (int i = 0; i < nums.length; i++) {
            sum = sum < 0 ? nums[i] : sum + nums[i]; // genuis!!         
            max = Math.max(max, sum);
        }
        
        return max;
    }
}
```



---

#### 763. Partition Labels

- Runtime: 3 ms, faster than 80.63% of Java online submissions for Partition Labels.

- Memory Usage: 38.3 MB, less than 5.19% of Java online submissions for Partition Labels.

use **int[26]** to record the last index of each character

```java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> count = new ArrayList<>();
        int[] map = new int[26];
        
        // record the last index of each character
        for (int i = 0; i < S.length(); i++) {
            map[S.charAt(i) - 'a'] = i;
        }
        
        int start = 0;
        int last = 0;
        for (int i = 0; i < S.length(); i++) {
            last = Math.max(last, map[S.charAt(i) - 'a']);
            if (last == i) {
                count.add(last - start + 1);
                start = last + 1;
            }
        }
        
        return count;
    }
}
```



---

 