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





