# 📕 Solution

```java
class Solution {
    public int answer;
    public void dfs(int count, int sum, int S, int[] nums, int size){
        if(count == size){
            if(sum == S){
                answer++;
            }
            return;
        }
        dfs(count + 1, sum + nums[count], S, nums, size);
        dfs(count + 1, sum - nums[count], S, nums, size);
    }
    public int findTargetSumWays(int[] nums, int S) {
        int size = nums.length;
        answer = 0;
        dfs(0, 0, S, nums, size);
        return answer;
    }
}
```
