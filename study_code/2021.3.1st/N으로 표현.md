# 📕 Solution

```java
class Solution {
    public int answer, n, num;
    public void dfs(int sum, int count){
        if(sum == num){
            if(answer > count) answer = count;
            return;
        }
        if(count > 8){
            return;
        }
        int n_continue = n;
        for(int i = 1; i < 9 - count; i++){
            dfs(sum + n_continue, count + i);
            dfs(sum - n_continue, count + i);
            dfs(sum * n_continue, count + i);
            dfs(sum / n_continue, count + i);    
            n_continue = 10 * n_continue + n;
        }
    }
    public int solution(int N, int number) {
        answer = 10;
        n = N;
        num = number;
        dfs(0, 0);
        if(answer > 8) answer = -1;
        return answer;
    }
}
```

```java
class Solution {
    public int answer, n, num;
    public void dfs(int count, int value) {
        if(value == num){
            answer = Math.min(answer, count);
            return;
        }
        if(count > 8){
            return;
        }
        int N_value = 0;
        for(int i = 1; i <= 8 - count; i++){
            N_value = 10 * N_value + n;
            dfs(count + i, value + N_value);
            dfs(count + i, value - N_value);
            dfs(count + i, value * N_value);
            dfs(count + i, value / N_value);
        }
    }
    public int solution(int N, int number) {
        answer = 9;
        n = N;
        num = number;
        dfs(0, 0);
        if(answer == 9){
            return -1;    
        } 
        return answer;
    }
}
```

## ❌ fail

.
