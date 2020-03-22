## Dynamic Programming



### 62. Unique Paths

- Runtime: **0 ms**

- Memory Usage: **36.6 MB**

**2D DP**

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i][j-1] + dp[i-1][j];
            }
        }
        
        return dp[m-1][n-1];
    }
}
```

**1D DP**

⚠️ : How to reduce the dimension from 2 to 1

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] row = new int[m];
        row[0] = 1;
        
        for (int i = 0; i < n; i++) {
            for (int j = 1; j < m; j++) {
                row[j] += row[j-1];
            }
        }
        
        return row[m-1];
    }
}
```

**Math Solution**

use [**Combination Formula**](https://baike.baidu.com/item/%E6%8E%92%E5%88%97%E7%BB%84%E5%90%88/706498)

```java
class Solution {
    public int uniquePaths(int m, int n) {
        if (m == 1 || n ==1) {
            return 1;
        }
        
        m--;
        n--;
        if (n < m) {
            int temp = n;
            n = m;
            m = temp;
        }
        
        int j = 1;
        long res = 1;
        for (int i = m + 1; i < m + n + 1; i++, j++){
            res *= i;
            res /= j;
        }
        
        return (int)res;
    }
}
```



---

### 63. Unique Path II

- Runtime: **0 ms**

- Memory Usage: **37.9 MB**

**1D DP**

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid[0].length;
        int n = obstacleGrid.length;
        int[] row = new int[m];
        row[0] = 1;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (obstacleGrid[i][j] == 1) {
                    row[j] = 0;
                } else if (j > 0) {
                    row[j] += row[j-1]; 
                }
            }
        }
        
        return row[m-1];
        
    }
}
```



---

### 198. House Robber

**DP Solution**

- Runtime: **0 ms**, faster than 100.00% of Java online submissions for House Robber.

- Memory Usage: **37 MB,** less than 5.26% of Java online submissions for House Robber.

```java
class Solution {
    public int rob(int[] nums) {
      	//dp[i][0]: not rob current house
      	//dp[i][1]: rob current house
        int[][] dp = new int[nums.length + 1][2];
        for (int i = 1; i <= nums.length; i++) {
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = nums[i-1] + dp[i-1][0];
        }
        
        return Math.max(dp[nums.length][1], dp[nums.length][0]); 
    }
}
```

**O(1) Solution**

- Runtime: **0 ms**, faster than 100.00% of Java online submissions for House Robber.

- Memory Usage: **37.1 MB**, less than 5.26% of Java online submissions for House Robber.

```java
class Solution {
    public int rob(int[] nums) {
        int preNo = 0;
        int preYes = 0;
        for (int n: nums) {
            int tmp = preNo;
            preNo = Math.max(preNo, preYes);
            preYes = n + tmp;
        }
        
        return Math.max(preNo, preYes);
    }
}
```



