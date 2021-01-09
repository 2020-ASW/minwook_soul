```java
class Solution {
    public int[] X = {1, 0, -1, 0};
    public int[] Y = {0, -1, 0, 1};
    public int m, n;
    public boolean[][] visited;
    public void dfs(int x, int y, char[][] grid){
        visited[x][y] = true;
        for(int i = 0; i < 4; i++){
            int nextX = x + X[i];
            int nextY = y + Y[i];
            if(nextX < 0 || nextX >= m){
                continue;
            }
            if(nextY < 0 || nextY >= n){
                continue;
            }
            if(grid[nextX][nextY] == '0'){
                visited[nextX][nextY] = true;
            }
            if(visited[nextX][nextY]){
                continue;
            }
            dfs(nextX, nextY, grid);
        }
        return;
    }
    public int numIslands(char[][] grid) {
        int answer = 0;
        m = grid.length;
        n = grid[0].length;
        visited = new boolean[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(!visited[i][j] && grid[i][j] == '1'){
                    dfs(i, j, grid);
                    answer++;
                }
            }
        }
        return answer;
    }
}
```
