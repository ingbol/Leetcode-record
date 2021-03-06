## DFS & BFS



### 79. Word Search 

- Runtime: **5 ms**, faster than 79.61% of Java online submissions for Word Search.
- Memory Usage: **43.1 MB**, less than 20.41% of Java online submissions for Word Search.

dfs

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

  dfs

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

dfs 与1091套路，循环模式基本一致

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

dfs

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



---

### 695. Max Area of Island

- Runtime: 3 ms, faster than 41.16% of Java online submissions for Max Area of Island.

- Memory Usage: 40 MB, less than 96.30% of Java online submissions for Max Area of Island.

bfs

```java
class Solution {
    private int m, n;
    private int[][] dirs = {{1, 0},{0, 1},{-1, 0},{0 ,-1}};
    
    public int maxAreaOfIsland(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        m = grid.length;
        n = grid[0].length;
        int maxArea = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                maxArea = Math.max(maxArea, dfs(grid, i, j));
            }
        }
        
        return maxArea;
    }
    
    private  int dfs(int[][] grid, int r, int c) {
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == 0) {
            return 0;
        }
        grid[r][c] = 0;
        int area = 1;
        for (int[] dir : dirs) {
            area += dfs(grid, r + dir[0], c + dir[1]);
        }
        return area;
    }
}
```



----

### 200. Number of Islands

- Runtime: 1 ms, faster than 99.98% of Java online submissions for Number of Islands.

- Memory Usage: 42.2 MB, less than 27.44% of Java online submissions for Number of Islands.

```java
class Solution {
    
    int m;
    int n;
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        m = grid.length;
        n = grid[0].length;
        int num = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    num++;
                    dfs(grid, i, j);
                }
            }
        }
         return num;
    }
    
    public void dfs(char[][] grid, int r, int c) {
        if(r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == '0') {
            return;
        }
        grid[r][c] = '0';
        for (int[] dir : dirs) {
            dfs(grid, r + dir[0], c + dir[1]);
        }
    }
}
```



---

### 547. Friend Circles

- Runtime: 0 ms, faster than 100.00% of Java online submissions for Friend Circles.

- Memory Usage: 40.6 MB, less than 60.00% of Java online submissions for Friend Circles.

这个题目比较特殊，值得复习。

```java
class Solution {
    int n;
    
    public int findCircleNum(int[][] M) {
        if (M == null || M.length == 0) {
            return 0;
        }
        
        n = M.length;
        int num = 0;
        boolean[] visited = new boolean[n]; //false
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                num++;
                dfs(M, i, visited);
            }
        }
        
        return num;
    }
    
    public void dfs(int[][] M, int i, boolean[] visited) {
        if (visited[i]) {
            return;
        }
        visited[i] = true;
        for(int k = 0; k < n; k++) {
            if (M[i][k] == 1) {
                dfs(M, k, visited);
            }
        }
    }
}
```



---

### 130. Surrounded Regions

- Runtime: 1 ms, faster than 99.81% of Java online submissions for Surrounded Regions.

- Memory Usage: 41.2 MB, less than 78.57% of Java online submissions for Surrounded Regions.

```java
class Solution {
    int m;
    int n;
    int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
   
    public void solve(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0 ) {
            return;
        }
        
        m = board.length;
        n = board[0].length;
        
        for (int i = 0; i < m; i++){
            dfs(board, i, 0);
            dfs(board, i, n -1);
        }
        for (int i = 0; i < n; i++) {
            dfs(board, 0, i);
            dfs(board, m - 1, i);
        }
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'T'){
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O'){
                    board[i][j] = 'X';
                }
            }
        }
    }  
    
    public void dfs(char[][] board, int r, int c) {
        if(r < 0|| r >= m || c < 0 || c >= n || board[r][c] != 'O'){
            return;
        }
        board[r][c] = 'T';
        for (int[] dir: dirs) {
            dfs(board, r + dir[0], c + dir[1]);
        }
    }
}
```

