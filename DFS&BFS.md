## DFS & BFS



### 79. Word Search

- Runtime: **5 ms**, faster than 79.61% of Java online submissions for Word Search.

- Memory Usage: **43.1 MB**, less than 20.41% of Java online submissions for Word Search.

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == word.charAt(0) && dfs(board, i, j, 0, word)){
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private boolean dfs(char[][] board, int i, int j, int count, String word) {
        if (count == word.length()) {
            return true;
        }
        
        if (i == -1 || i == board.length || j == -1 || j == board[0].length || board[i][j] != word.charAt(count)) {
            return false;
        }
        
        char temp = board[i][j];
        board[i][j] = ' ';
        boolean found = dfs(board, i - 1, j, count + 1, word) ||
                        dfs(board, i + 1, j, count + 1, word) ||
                        dfs(board, i, j - 1, count + 1, word) ||
                        dfs(board, i, j + 1, count + 1, word);
        board[i][j] = temp;
        
        return found;
        
    }
}
```



---

### 1091. Shortest Path in Binary Matrix

- Runtime: 28ms
- Memory: 41.9 mb

```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return -1;
        }
        
        int[][] dir = new int[][]{{-1, 0}, {0, -1}, {0, 1}, {1, 0}, {-1, -1}, {1, -1}, {-1, 1}, {1, 1}};
        Queue<Pair<Integer, Integer>> path = new LinkedList<>();
        path.add(new Pair(0,0));
        
        int m = grid.length; //height
        int n = grid[0].length; //width
        
        int length = 0; //path length
        
        while (!path.isEmpty()){
            length++;
            int size = path.size();
            while (size-- > 0) {
                Pair<Integer, Integer> cur = path.poll();
                int cr = cur.getKey(); // current row
                int cc = cur.getValue(); // current column
                if (grid[cr][cc] == 1) {
                    continue;
                }
                if (cr == m - 1 && cc == n - 1) {
                    return length;
                }
                
                grid[cr][cc] = 1; // label passed position
                
                for (int[] d : dir) {
                    if (cr + d[0] >= m || cr + d[0] < 0 || cc + d[1] >= n || cc + d[1] < 0) {
                        continue;
                    }
                    int nr = cr + d[0]; // new row
                    int nc = cc + d[1]; // new column
                    
                    path.add(new Pair(nr, nc));
                }
            }
        }
        
        return -1;
    }
}
```



---

### 279. Perfect Squares

- Runtime: 18 ms, faster than 93.61% of Java online submissions for Perfect Squares.

- Memory Usage: 39.4 MB, less than 13.89% of Java online submissions for Perfect Squares.

与1091套路，循环模式基本一致

```java
class Solution {
    public int numSquares(int n) {
        if (n == 0) {
            return 0;
        }
        
        List<Integer> squares = generateSquares(n);
        Queue<Integer> queue = new LinkedList<>();
        boolean[] marked = new boolean[n + 1];       
        int level = 0; //square number 的数量
        
        queue.add(n);
        marked[n] = true;
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            level++;
            while (size-- > 0) {
                int curNo = queue.poll();
                for (int s : squares) {
                    int nextNo = curNo - s;
                    if (nextNo < 0) {
                        break;
                    }
                    if (nextNo == 0) {
                        return level; 
                    }
                    if (marked[nextNo]) {
                        continue;
                    }
                    marked[nextNo] = true;
                    queue.add(nextNo);
                }
            }
            
        }
        
        return -1;
    }
    
    private List<Integer> generateSquares(int n) { //获得不大于n的平方数
        List<Integer> squares = new ArrayList<>();
        int curNo = 1;
        int square = 1;
        
        while (square <= n) {
            squares.add(square);
            curNo++;
            square = curNo * curNo;
        }
        
        return squares;
    }
}
```



---

### 127. Word Ladder

- Runtime: 12 ms, faster than 98.10% of Java online submissions for Word Ladder.

- Memory Usage: 40.2 MB, less than 77.37% of Java online submissions for Word Ladder

若使用297和1091的套路，使用Queue，runtime达1000ms+

这里使用了Two-end BFS，最大可能减少了循环的次数

使用**HashSe**t大大提高了程序运行速度，因为相对于ArrayList/LinkedList HashSet的contains方法可以直接通过Hashcode找到元素位置，而ArrayList/LinkedList都需要遍历整个List直到找到该元素为止。

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        if (!wordList.contains(endWord)) {
            return 0;
        }
        
        Set<String> wordSet = new HashSet<>(wordList);
        Set<String> beginSet = new HashSet<>();
        Set<String> endSet = new HashSet<>();
        
        beginSet.add(beginWord);
        endSet.add(endWord);
        wordSet.remove(endWord);
        
        int count = 1;
        while (!beginSet.isEmpty()) {
            count++;
            Set<String> nextSet = new HashSet<>();
            for (String word : beginSet) {
                char[] wordArray = word.toCharArray();
                for(int i = 0; i < wordArray.length; i++) {
                    char old = wordArray[i]; //暂存第i位char
                    
                    for (char c = 'a'; c <= 'z'; c++) {
                        wordArray[i] = c;
                        String newWord = String.valueOf(wordArray);
                        if (endSet.contains(newWord)){
                            return count;
                        }
                        if (wordSet.contains(newWord)) {
                            nextSet.add(newWord);
                            wordSet.remove(newWord);
                        }
                    }
                    
                    wordArray[i] = old; //还原第i位
                }
            }
            
            //beginSet要小，endSet要大，这样可以减少循环
            beginSet = nextSet.size() < endSet.size() ? nextSet : endSet;
            endSet = beginSet.size() < endSet.size() ? endSet : nextSet;
        }
        
        return 0;
    }
}
```

