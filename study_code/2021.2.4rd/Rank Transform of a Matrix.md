# 📕 Solution

## 📑 문제 설명

1. Given an m x n matrix, return a new matrix answer where answer[row][col] is the rank of matrix[row][col].

2. The rank is an integer that represents how large an element is compared to other elements. It is calculated using the following rules:

3. The rank is an integer starting from 1.
   </br> If two elements p and q are in the same row or column, then:
   </br> 1. If p < q then rank(p) < rank(q)
   </br> 2. If p == q then rank(p) == rank(q)
   </br> 3. If p > q then rank(p) > rank(q)
   </br> The rank should be as small as possible.
   </br> It is guaranteed that answer is unique under the given rules.

## 👻 접근 방식

1. 행렬에 있는 모든 원소를 (x, y, 해당 좌표에 대한 원소값)으로 저장

2. 원소값에 따라 sort해줍니다.

3. 각 행과 열의 최댓값을 갱신해줄 수 있는 배열을 만들어줍니다.

4. 새로운 배열(=answer[row][col])에 넣어주면서 각 행과 열에 대한 최댓값이 같으면 해당 배열에 저장된 최대 rank를 추가해줍니다. 만약 해당 원소값이 각 행과 열에 대한 최댓값보다 크다면 최대 rank + 1를 추가해줍니다.

```java
int[] row = new int[row_size];
int[] column = new int[column_size];
int[] row_maxValue = new int[row_size];
int[] column_maxValue = new int[column_size];
int arr_size = arr.size();
for(int i = 0; i < arr_size; i++){
    Pair node = arr.get(i);
    int x = node.x;
    int y = node.y;
    int value = node.value;
    if(i == 0){
        answer[x][y] = Math.max(row[x], column[y]) + 1;
    } else {
        if(row_maxValue[x] == value || column_maxValue[y] == value){
            answer[x][y] = Math.max(row[x], column[y]);
        } else {
            answer[x][y] = Math.max(row[x], column[y]) + 1;
        }
    }
    row[x] = answer[x][y];
    column[y] = answer[x][y];
    row_maxValue[x] = value;
    column_maxValue[y] = value;
}
```

## ❌ fail

Input:
`[[-37,-50,-3,44],[-37,46,13,-32],[47,-42,-3,-40],[-17,-22,-39,24]]`  
Output:
`[[2,1,3,6],[2,6,5,4],[5,2,3,3],[4,3,1,5]]`  
Expected:
`[[2,1,4,6],[2,6,5,4],[5,2,4,3],[4,3,1,5]]`

(0,2)의 -3과 (2, 2)의 -3에 대해서 서로 rank가 갱신이 안되어 있음을 알 수 있습니다. 😢

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
        int[] row = new int[row_size];
        int[] column = new int[column_size];
        int[] row_maxValue = new int[row_size];
        int[] column_maxValue = new int[column_size];
        int arr_size = arr.size();
        for(int i = 0; i < arr_size; i++){
            Pair node = arr.get(i);
            int x = node.x;
            int y = node.y;
            int value = node.value;
            if(i == 0){
                answer[x][y] = Math.max(row[x], column[y]) + 1;
            } else {
                if(row_maxValue[x] == value || column_maxValue[y] == value){
                    answer[x][y] = Math.max(row[x], column[y]);
                    // 이 부분에서 다양한 예외 사항들이 존재합니다.
                } else {
                    answer[x][y] = Math.max(row[x], column[y]) + 1;
                }
            }
            row[x] = answer[x][y];
            column[y] = answer[x][y];
            row_maxValue[x] = value;
            column_maxValue[y] = value;
        }
        return answer;
    }
}
```

## 💪 풀이 방법

**Union-find를 이용해야만 합니다!**

1. 해당 원소값을 key, 원소에 대한 (x, y) 좌표값을 value로 하는 map을 만듭니다.

2. 해당 열과 행의 rank를 저장하는 `int[] Rank = new int[n + m];`와 Union-find를 해서 grouping을 할 `int[] isParent = new int[n + m];`을 선언해줍니다. </br> 0 ~ m - 1까지 행에 대한 값, m ~ mm + n - 1까지 열에 대한 값입니다.

3. 해당 원소값에 따라 root를 찾아 갱신해줍니다.

```java
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
        // 값이 index인 좌표에 대한 행, 열의 Rank 최댓값을 갱신해줍니다.
    }
}
```

4. 원소값이 grouping이 된 부분을 `matrix[x][y] = isRank[find(parent, x)] + 1;`를 해줍니다. </br> 값이 index인 좌표에 대한 행, 열의 Rank 최댓값 보다 1 큰 값이 `The rank should be as small as possible.`의 조건에 만족합니다.

```java
for(Pair pair : map.get(index)){
    int x = pair.x;
    int y = pair.y;
    matrix[x][y] = isRank[find(parent, x)] + 1;
    Rank[x] = matrix[x][y];
    Rank[y + m] = matrix[x][y];
}
```

## 👻 About

**union-find의 과정을 정확히 봅시다!**

## ✈ Explain Code

```java
import java.util.*;

class Pair {
    int x;
    int y;
    Pair(int x, int y){
        this.x = x;
        this.y = y;
    }
} // 좌표값 설정

class Solution {
    public int find(int[] parent, int x){
        while(parent[x] != x){
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x; // parent 값에 따른 grouping
    }
    public int[][] matrixRankTransform(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        TreeMap<Integer, ArrayList<Pair>> map = new TreeMap<>();
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                map.putIfAbsent(matrix[i][j], new ArrayList<>());
                map.get(matrix[i][j]).add(new Pair(i, j));
            } // 해당 원소값을 key, 원소에 대한 (x, y) 좌표값
        }
        int[] Rank = new int[n + m];
        int[] isParent = new int[n + m];
        for(int i = 0; i < n + m; i++){
            isParent[i] = i;
        } // parent 배열 초기화
        for(Integer index : map.keySet()){
            int[] parent = isParent.clone();
            int[] isRank = Rank.clone();
            // index에 대한 계산이 각자 적용될 수 있도록 합니다
            for(Pair pair : map.get(index)){
                int x = pair.x;
                int y = pair.y;
                int rootX = find(parent, x); // grouping의 root x값 찾기
                int rootY = find(parent, y + m); // grouping의 root y값 찾기
                parent[rootX] = rootY;
                isRank[rootY] = Math.max(isRank[rootX], isRank[rootY]);
                // 값이 index인 좌표에 대한 행, 열의 Rank 최댓값을 갱신해줍니다.
            }
            for(Pair pair : map.get(index)){
                int x = pair.x;
                int y = pair.y;
                matrix[x][y] = isRank[find(parent, x)] + 1;
                // 갱신한 값이 index인 좌표에 대한 행, 열의 Rank 최댓값 + 1을
                // 값이 index인 좌표에 넣어줍니다.
                Rank[x] = matrix[x][y];
                Rank[y + m] = matrix[x][y];
                // rank값 갱신
            }
        }
        return matrix;
    }
}
```

## ✈ Source Code

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

## ✔ 회고

union-find 다시 풀어봐야겠다....🤦‍♂️
