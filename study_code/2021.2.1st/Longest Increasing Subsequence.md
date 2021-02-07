# 📕 Solution

O(N^2) 방식

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int size = nums.length, answer = 0;
        int[] dp = new int[size];
        for(int i = 0; i < size; i++){
            dp[i] = 1;
            for(int j = 0; j < i; j++){
                if(nums[i] > nums[j]){
                   dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            answer = Math.max(answer, dp[i]);
        }
        return answer;
    }
}
```

## ❌ fail

```java

```
