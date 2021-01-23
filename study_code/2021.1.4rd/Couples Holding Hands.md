# 📕 Solution

```java
class Solution {
    public int minSwapsCouples(int[] row) {
        int Size = row.length, arr_size = Size / 2, answer = 0, num, temp;
        int[][] arr = new int[arr_size][2];
        for(int i = 0; i < arr_size; i++){
            arr[i][0] = row[2 * i];
            arr[i][1] = row[2 * i + 1];
        }
        for(int i = 0; i < arr_size; i++){
            if(arr[i][0] % 2 == 1){
                num = arr[i][0] - 1;
            } else {
                num = arr[i][0] + 1;
            }
            if(arr[i][1] == num){
                continue;
            }
            for(int j = i + 1; j < arr_size; j++){
                if(arr[j][0] == num){
                    temp = arr[j][0];
                    arr[j][0] = arr[i][1];
                    arr[i][1] = temp;
                    answer++;
                    break;
                }
                if(arr[j][1] == num){
                    temp = arr[j][1];
                    arr[j][1] = arr[i][1];
                    arr[i][1] = temp;
                    answer++;
                    break;
                }
            }
        }
        return answer;
    }
}
```
