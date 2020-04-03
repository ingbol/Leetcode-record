

## MISC

This page records some problems which can solved without specific algorithms.



### 122. Best Time to Buy and Sell Stock II

- Runtime: **1 ms**

- Memory Usage: **42.5 MB**

```java
class Solution {
    public int maxProfit(int[] prices) {
        int i = 0, buy, sell, profit = 0, N = prices.length - 1;
        while( i < N ){
          	//buy stock when the price is a local min
            while( i < N && prices[i+1] <= prices[i] ) i++;
            buy = prices[i];
            
          	//sell stock when the price is a local max
            while( i < N && prices[i+1] > prices[i] ) i++;
            sell = prices[i];
            
            profit += sell - buy;
        }
        
        return profit;
    }
}
```



---

### 1029. Two City Scheduling

- Runtime: **1 ms**

- Memory Usage: **37.7 MB**

**sort solution**

```java
class Solution {
    public int twoCitySchedCost(int[][] costs) {
        Arrays.sort(costs, new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                return (a[0] - b[0]) - (a[1] - b[1]);
            }
        });
        
        int cost = 0;
        for(int i = 0; i < costs.length / 2; i++){
            cost += (costs[i][0] + costs[costs.length - i - 1][1]);
        }
        return cost;
    }
}
```



---

### 991. Broken Calculator

- Runtime: **0 ms**

- Memory Usage: **36.6 MB**

**Solution 1** : consider Y -> X

```java
class Solution {
    public int brokenCalc(int X, int Y) {
        int ans = 0;
        
        while(Y > X){
            Y = Y % 2 > 0 ? Y + 1 : Y / 2;
            ans++;
        }
        
        return ans + X - Y;
    }
}
```

**Solution 2** : Recursion

```java
class Solution {
    public int brokenCalc(int X, int Y) {
        if( X >= Y) return X - Y;
        
        return 1 + (Y % 2 == 0? brokenCalc(X, Y / 2): brokenCalc(X, Y + 1));
    }
}
```



---





