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

