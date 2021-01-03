```java
import java.util.*;

class Solution {
    private List<List<String>> result;
    public boolean isPoosible(int count, int num, int[] answer, int n){
        for(int i = 0; i < count; i++){
            if(Math.abs(i - count) == Math.abs(answer[i] - num)){
                return false;
            } // check diagonal
            if(Math.abs(answer[i] - num) == 0){
                return false;
            } // check row
        }
        return true;
    }
    public void dfs(int count, int[] answer, int n){
        if(count == n){
            result.add(Change(answer));
            return;
        }
        for(int i = 0; i < n; i++){
            if(isPoosible(count, i, answer, n)){
                answer[count] = i;
                dfs(count + 1, answer, n);
            }
        }
    }
    public ArrayList<String> Change(int[] answer){
        ArrayList<String> str = new ArrayList<String>();
        String word = "";
        for(Integer element : answer){
            word = "";
            for(int i = 0; i < element; i++){
                word += ".";
            }
            word += "Q";
            for(int i = element + 1; i < answer.length; i++){
                word += ".";
            }
            str.add(word);
        }
        return str;
    }
    public List<List<String>> solveNQueens(int n) {
        result = new ArrayList<>();
        int[] answer = new int[n];
        dfs(0, answer , n);
        return result;
    }
}
```
