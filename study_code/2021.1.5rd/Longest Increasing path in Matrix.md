# 📕 Solution

```java
class Solution {
    public int row, column;
    public final int[] X = {1, 0, -1, 0};
    public final int[] Y = {0, 1, 0, -1};
    public int dfs(int x, int y, int[][] matrix, int[][] visited){
        if(visited[x][y] != 0) return visited[x][y];
        int res = 1;
        for(int i = 0; i < 4; i++){
            int nextX = x + X[i];
            int nextY = y + Y[i];
            if(nextX < 0 || nextX >= row){
                continue;
            }
            if(nextY < 0 || nextY >= column){
                continue;
            }
            if(matrix[x][y] < matrix[nextX][nextY]){
                res = Math.max(res, dfs(nextX, nextY, matrix, visited) + 1);
            }
        }
        visited[x][y] = res;
        return visited[x][y];
    }
    public int longestIncreasingPath(int[][] matrix) {
        row = matrix.length;
        column = matrix[0].length;
        int answer = 0;
        int[][] visited = new int[row][column];
        for(int i = 0; i < row; i++){
            for(int j = 0; j < column; j++){
                answer = Math.max(answer, dfs(i, j, matrix, visited));
            }
        }
        return answer;
    }
}
```

## ❌ fail

문제점 : 시간초과

```java
class Solution {
    public int result, row, column;
    public final int[] X = {1, 0, -1, 0};
    public final int[] Y = {0, 1, 0, -1};
    public void dfs(int x, int y, int[][] matrix, boolean[][] visited, int count){
        visited[x][y] = true;
        for(int i = 0; i < 4; i++){
            int nextX = x + X[i];
            int nextY = y + Y[i];
            if(nextX < 0 || nextX >= row){
                continue;
            }
            if(nextY < 0 || nextY >= column){
                continue;
            }
            if(visited[nextX][nextY]){
                continue;
            }
            if(matrix[x][y] < matrix[nextX][nextY]){
                visited[nextX][nextY] = true;
                dfs(nextX, nextY, matrix, visited, count + 1);
                visited[nextX][nextY] = false;
            }
        }
        if(result < count) result = count;
    }
    public boolean[][] clear(boolean[][] visited){
        for(int i = 0; i < row; i++){
            for(int j = 0; j < column; j++){
                visited[i][j] = false;
            }
        }
        return visited;
    }
    public int longestIncreasingPath(int[][] matrix) {
        row = matrix.length;
        column = matrix[0].length;
        int answer = 0;
        boolean[][] visited = new boolean[row][column];
        for(int i = 0; i < row; i++){
            for(int j = 0; j < column; j++){
                result = 0;
                clear(visited);
                dfs(i, j, matrix, visited, 1);
                if(answer < result) answer = result;
            }
        }
        return answer;
    }
}
```
