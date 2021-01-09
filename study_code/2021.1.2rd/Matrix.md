# 📕 Solution

```java
class Solution {
    public int m, n;
    public int[][] dp;
    public void UpdateRight(){
        for(int i = 0; i < m; i++){
            for(int j = 1; j < n; j++){
                dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
            }
        }
    }
    public void UpdateLeft(){
        for(int i = 0; i < m; i++){
            for(int j = n - 2; j >= 0; j--){
                dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
            }
        }
    }
    public void UpdateUp(){
        for(int i = m - 2; i >= 0; i--){
            for(int j = 0; j < n; j++){
                dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
            }
        }
    }
    public void UpdateDown(){
        for(int i = 1; i < m; i++){
            for(int j = 0; j < n; j++){
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
            }
        }
    }
    public int[][] updateMatrix(int[][] matrix) {
        m = matrix.length;
        n = matrix[0].length;
        dp = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(matrix[i][j] == 0){
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = 10002;
                }
            }
        }
        UpdateRight();
        UpdateLeft();
        UpdateUp();
        UpdateDown();
        return dp;
    }
}
```

# ❌ Matrix

문제점 : 한점마다 O(N^2)을 하고 있으므로 시간 초과가 납니다.

```java
class Solution {
    public int m, n;
    public int[][] dp;
    public boolean[][] visited;
    public int Check(int x, int y, int[][] matrix){
        int answer = 0, distance = 1;
        while(true){
            for(int i = -distance; i <= distance; i++){
                for(int j = -distance; j <= distance; j++){
                    if(x + i < 0 || x + i >= m){
                        continue;
                    }
                    if(y + j < 0 || y + j >= n){
                        continue;
                    }
                    if(i + j == distance){
                        if(matrix[x + i][y + j] == 0){
                            return distance;
                        }
                        if(visited[x + i][y + j]){
                            return distance + dp[x + i][y + j];
                        }
                    }
                }
            }
            distance++;
        }
    }
    public int[][] updateMatrix(int[][] matrix) {
        m = matrix.length;
        n = matrix[0].length;
        dp = new int[m][n];
        visited = new boolean[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(matrix[i][j] == 0){
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = Check(i, j, matrix);
                }
                visited[i][j] = true;
            }
        }
        return dp;
    }
}
```
