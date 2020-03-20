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



