# 📕 Solution

## :memo: 문제 설명

1. Given an m x n 2d grid map of '1's (land) and '0's (water)
2. return the number of islands.

## 👻 접근 방식 and 💪 풀이 방법

옛날 nhn pre-test에서 나온 문제여서 보고 바로 알 수 있었습니다.  
**참고 사이트 :** [Go to link](https://recruit.nhn.com/pdf/%ED%94%84%EB%A6%AC%ED%85%8C%EC%8A%A4%ED%8A%B8_1%EC%B0%A8_%EA%B8%B0%EC%B6%9C%EB%AC%B8%EC%A0%9C.pdf)

1. `int[] X = {1, 0, -1, 0};` 와 `int[] Y = {0, 1, 0, -1};`로 상하좌우로 갈 수 있는 경로를 설정합니다.
2. `visited[m][n]`를 선언해서 방문여부를 저장해줍니다.
3. 이전에 방문한 노드를 `visited[][] = true`로 바꿔서 재방문을 방지합니다.

## ✈ Explain Code

```java
class Solution {
    public int[] X = {1, 0, -1, 0}; // 방향 설정
    public int[] Y = {0, -1, 0, 1}; // 방향 설정
    public int m, n;
    public boolean[][] visited;
    public void dfs(int x, int y, char[][] grid){
        visited[x][y] = true; // 재방문 방지
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
            } // water인 경우 visited를 true로 설정
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
                    // 방문하지 않은land인 경우에만 dfs를 확인합니다.
                    answer++;
                }
            }
        }
        return answer;
    }
}
```

## ✈ Source Code

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

## ✔ 회고

이것만 풀었어도....
