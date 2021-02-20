# 📕 Solution

## :memo: 문제 설명

1. n \* n 배열이 주어지는데, 행과 열이 오름차순으로 정렬되어 있습니다. </br> Given an n x n matrix where each of the rows and columns are sorted in ascending order,

2. 행렬 안에서 k번째 작은 원소를 찾아 return합니다. </br> return the kth smallest element in the matrix. </br> Note that it is the kth smallest element in the sorted order, not the kth distinct element.

## 💪 풀이 방법

1. 오름차순으로 정렬할 list 하나를 지정합니다.

2. 해당 배열을 기준으로 왼쪽 아래 방향에서 오른쪽 위 방향으로 대각선으로 그어 대각선에 있는 원소들을 순서대로 확인해봅니다.

3. 만략 대각선 안에 있는 원소들이 list의 k번째 원소보다 다 클 경우, list의 k번째 원소를 반환합니다. </br> 아닐 경우, 그 다음 대각선를 확인해봅니다.

Ex)

|     |     |     |
| :-: | :-: | :-: |
|  1  |  5  |  9  |
| 10  | 11  | 13  |
| 12  | 13  | 15  |

`[1]` -> `[5, 10]` -> `[9, 11, 12]` -> `[13, 13]` -> `[15]` 순으로 확인합니다.

## ✈ Explain Code

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> list;
    public int BinarySearch(int num){
        int left = 0;
        int right = list.size() - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            if(list.get(mid) < num){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    } // 이분 탐색으로 해당 num이 오름차순으로 정렬될 때, 어디에 들어가는지 확인합니다.

    public int kthSmallest(int[][] matrix, int k) {
        list = new ArrayList<Integer>();
        int size = matrix.length, total_size = 2 * (size - 1);
        for(int total = 0; total <= total_size; total++){
            int y = (total < size) ? total : size - 1;
            int x = total - y;
            // 왼쪽 아래 방향에서 오른쪽 위 방향으로 천천히 확인해봅니다.
            boolean isComplete = true;
            // 해당 대각선 안에 있는 원소들이 list의 k번째 원소보다 크면 true, 아니면 false
            for(int j = x; j <= y; j++){
                int num = BinarySearch(matrix[j][total - j]);
                if(num < k && isComplete) isComplete = false;
                list.add(num, matrix[j][total - j]);
            }
            if(isComplete){
                break;
            }
        }
        return list.get(k - 1);
    }
}
```

## ✈ Source Code

```java
import java.util.*;

class Solution {
    public ArrayList<Integer> list;
    public int BinarySearch(int num){
        int left = 0;
        int right = list.size() - 1;
        while(left <= right){
            int mid = (left + right) / 2;
            if(list.get(mid) < num){
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
    public int kthSmallest(int[][] matrix, int k) {
        list = new ArrayList<Integer>();
        int size = matrix.length, total_size = 2 * (size - 1);
        for(int total = 0; total <= total_size; total++){
            int y = (total < size) ? total : size - 1;
            int x = total - y;
            boolean isComplete = true;
            for(int j = x; j <= y; j++){
                int num = BinarySearch(matrix[j][total - j]);
                if(num < k && isComplete) isComplete = false;
                list.add(num, matrix[j][total - j]);
            }
            if(isComplete){
                break;
            }
        }
        return list.get(k - 1);
    }
}
```

## ✔ 회고

이전에 풀었던 내용이여서 쉽게 풀었습니다!👍👍
