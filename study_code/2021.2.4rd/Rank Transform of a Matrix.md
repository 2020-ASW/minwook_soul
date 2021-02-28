# 📕 Solution

```java
import java.util.*;

class Pair {
    int x;
    int y;
    Pair(int x, int y){
        this.x = x;
        this.y = y;
    }
}

class Solution {
    public int find(int[] parent, int x){
        while(parent[x] != x){
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }
    public int[][] matrixRankTransform(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        TreeMap<Integer, ArrayList<Pair>> map = new TreeMap<>();
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                map.putIfAbsent(matrix[i][j], new ArrayList<>());
                map.get(matrix[i][j]).add(new Pair(i, j));
            }
        }
        int[] Rank = new int[n + m];
        int[] isParent = new int[n + m];
        for(int i = 0; i < n + m; i++){
            isParent[i] = i;
        }
        for(Integer index : map.keySet()){
            int[] parent = isParent.clone();
            int[] isRank = Rank.clone();
            for(Pair pair : map.get(index)){
                int x = pair.x;
                int y = pair.y;
                int rootX = find(parent, x);
                int rootY = find(parent, y + m);
                parent[rootX] = rootY;
                isRank[rootY] = Math.max(isRank[rootX], isRank[rootY]);
            }
            for(Pair pair : map.get(index)){
                int x = pair.x;
                int y = pair.y;
                matrix[x][y] = isRank[find(parent, x)] + 1;
                Rank[x] = matrix[x][y];
                Rank[y + m] = matrix[x][y];
            }
        }
        return matrix;
    }
}
```

## ❌ fail

```java
import java.util.*;

class Pair {
    int x;
    int y;
    int value;
    Pair(int x, int y, int value){
        this.x = x;
        this.y = y;
        this.value = value;
    }
}

class Solution {
    public int[][] matrixRankTransform(int[][] matrix) {
        int row_size = matrix.length, column_size = matrix[0].length;
        int[][] answer = new int[row_size][column_size];
        int[] row = new int[row_size];
        int[] column = new int[column_size];
        ArrayList<Pair> arr = new ArrayList<Pair>();
        for(int i = 0; i < row_size; i++){
            for(int j = 0; j < column_size; j++){
                arr.add(new Pair(i, j, matrix[i][j]));
            }
        }
        Collections.sort(arr, new Comparator<Pair>(){
            @Override
            public int compare(Pair o1, Pair o2){
                return o1.value - o2.value;
            }
        });
        int arr_size = arr.size(), before = 0;
        for(int i = 0; i < arr_size; i++){
            Pair node = arr.get(i);
            if(i == 0 || before != node.value){
                answer[node.x][node.y] = Math.max(row[node.x], column[node.y]) + 1;
            } else {
                answer[node.x][node.y] = Math.max(row[node.x], column[node.y]);
            }
            row[node.x] = answer[node.x][node.y];
            column[node.y] = answer[node.x][node.y];
            before = node.value;
        }
        return answer;
    }
}
```
