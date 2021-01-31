# 📕 Solution

```java
import java.util.*;

class Solution {
    public int jump(int[] nums) {
        int size = nums.length;
        int[] count = new int[size];
        boolean[] visited = new boolean[size];
        Arrays.fill(count, Integer.MAX_VALUE);
        Queue<Integer> queue = new LinkedList<Integer>();
        count[0] = 0;
        queue.offer(0);
        visited[0] = true;
        while(!queue.isEmpty()){
            int node = queue.poll();
            for(int i = 1; i <= nums[node]; i++){
                if(node + i < size && !visited[node + i] && count[node + i] > count[node] + 1){
                    count[node + i] = count[node] + 1;
                    queue.offer(node + i);
                    visited[i] = true;
                }
            }
        }
        return count[size - 1];
    }
}
```
