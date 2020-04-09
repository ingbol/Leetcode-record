## Divide and Conquer



### 241. Different Ways to Add Parentheses

- Runtime: 1 ms, faster than 97.86% of Java online submissions for Different Ways to Add Parentheses.

- Memory Usage: 40 MB, less than 66.67% of Java online submissions for Different Ways to Add Parentheses.

```java
class Solution {
    public List<Integer> diffWaysToCompute(String input) {
        List<Integer> ways = new ArrayList<>();
        
        for (int i = 0; i < input.length(); i++) {
            char c = input.charAt(i);
            if (c == '+' || c =='-' || c == '*') {
                List<Integer> left = diffWaysToCompute(input.substring(0, i));
                List<Integer> right = diffWaysToCompute(input.substring(i + 1));
                
                for (int l : left) {
                    for (int r : right) {
                        switch (c) {
                            case '+' :
                                ways.add(l + r);
                                break;
                            case '-' :
                                ways.add(l - r);
                                break;
                            case '*' :
                                ways.add(l * r);
                                break;
                            default:
                                break;
                        }
                    }
                }
            }
        }
        
      	// only contains 1 number
        if (ways.size() == 0) {
            ways.add(Integer.valueOf(input));
        }
        return ways;
    }
}
```

