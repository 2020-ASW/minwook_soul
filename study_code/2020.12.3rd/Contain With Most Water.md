```java
class Solution {
    public int maxArea(int[] height) {
        int max = 0, start = 0, end = height.length - 1;
        while(end > start){
            int num = (height[start] > height[end]) ? height[end] : height[start];
            int size = num * (end - start);
            if(height[start] < height[end]){
                start += 1;
            } else {
                end -= 1;
            }
            max = (size > max) ? size : max;
        }
        return max;
    }
}
```
