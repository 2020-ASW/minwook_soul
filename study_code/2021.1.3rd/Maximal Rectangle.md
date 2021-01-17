```java
import java.util.*;

class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0) return 0;
        int x_size = matrix.length, y_size = matrix[0].length, Max = 0;
        int[][] dp = new int[x_size][y_size + 1];
        for(int i = 0; i < x_size; i++){
            for(int j = 0; j < y_size; j++){
                dp[i][j] = matrix[i][j] - '0';
                if(i > 0 && dp[i][j] != 0){
                    dp[i][j] += dp[i - 1][j];
                }
            }
            Stack<Integer> stack = new Stack<Integer>();
            for(int j = 0; j <= y_size; j++){
                if(stack.empty()){
                    stack.push(j);
                    continue;
                }
                while(!stack.empty() && dp[i][stack.peek()] >= dp[i][j]){
                    int height = dp[i][stack.pop()], area;
                    if(stack.empty()){
                        area = height * j;
                    } else {
                        area = height * (j - stack.peek() - 1);
                    }
                    if(Max < area) Max = area;
                }
                stack.push(j);
            }
        }
        return Max;
    }
}
```
