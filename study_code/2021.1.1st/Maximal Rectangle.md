# 📕 Solution

참고 사이트 : [Go to link](https://ohgym.tistory.com/90)

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0) return 0;
        int x_size = matrix.length, y_size = matrix[0].length, Max = 0;
        int[][] dp = new int[x_size][y_size];
        int[] height = new int[y_size + 1];
        for(int i = 0; i < x_size; i++){
            Stack<Integer> stack = new Stack<Integer>();
            for(int j = 0; j <= y_size; j++){
                if(j == y_size || matrix[i][j] == '0'){
                    height[j] = 0;
                } else {
                    height[j]++;
                }
                while(!stack.isEmpty() && height[stack.peek()] >= height[j]){
                    int height_peek = height[stack.peek()];
                    stack.pop();
                    int width = stack.isEmpty() ? j : j - stack.peek() - 1;
                    if(Max < height_peek * width) Max = height_peek * width;
                }
                stack.push(j);
            }
        }
        return Max;
    }
}
```
