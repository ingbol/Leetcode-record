## Graph



### 1202. Smallest String With Swaps

**Union Find Solution**

```java
class Solution {
    //Union Find Solution
    private class UnionFind {
        
        public int[] ranks;
        public int[] parents;
     
        UnionFind(int count) {
            ranks = new int[count];
            parents = new int[count];
            for (int i = 0; i < count; i++) {
                ranks[i] = 1;
                parents[i] = i;
            }
        }
        
        int find(int p) {
            if (p != parents[p]) {
                parents[p] = find(parents[p]);
            }
            return parents[p];
        }
        
        int union(int p, int q) {
            int pRoot = find(p);
            int qRoot = find(q);
            if ( pRoot == qRoot) {
                return ranks[pRoot];
            }
            if (ranks[pRoot] > ranks[qRoot]) {
                parents[qRoot] = pRoot;
                ranks[pRoot] += ranks[qRoot];
                
                return ranks[pRoot];
            } else {
                parents[pRoot] = qRoot;
                ranks[qRoot] += ranks[pRoot];
                
                return ranks[qRoot];
            }
        }
    }
    
    public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
        UnionFind uf = new UnionFind(s.length());
        //initialize parent nodes
        for (int i = 0; i < uf.parents.length; i++) {
            uf.parents[i] = i;
        }
        
        for (List<Integer> pair : pairs) {
            uf.union(pair.get(0), pair.get(1));
        }
        
        Map<Integer, PriorityQueue<Character>> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            int root = uf.find(i);
            map.putIfAbsent(root, new PriorityQueue<>());
            map.get(root).offer(s.charAt(i));
        }
        
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            sb.append(map.get(uf.find(i)).poll());
        }

        return sb.toString();
    }
    
}
```

