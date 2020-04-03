

## MISC

This page records some problems which can solved without specific algorithms.



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





