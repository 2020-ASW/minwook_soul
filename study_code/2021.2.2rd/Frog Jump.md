# 📕 Solution

## :memo: 문제 설명

1. A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.

2. Given a list of stones' positions (in units) in sorted ascending order, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be 1 unit.

3. If the frog's last jump was k units, its next jump must be either k - 1, k, or k + 1 units. The frog can only jump in the forward direction.

## 💪 풀이 방법

dp라고 생각했지만, 알아야 하는 것이 해당 위치(=pos), 이전에 움직인 크기(=k), 해당 위치가 가능한지 확인할 수 있는 boolean값이 필요합니다.

```java
class Solution {
public:
   map<int, bool> stone;
   int lastPos;
   map<pair<int, int>, bool> dp;
   bool solve(int pos, int k) {
      if (pos == lastPos)
         return true;
      if (k <= 0 || pos > lastPos)
         return false;
      if (dp.find({pos, k}) != dp.end())
         return dp[{pos, k}];
      if (stone[pos] == 1)
         return dp[{pos, k}] = solve(pos + k, k) ||
                               solve(pos + k - 1, k - 1) ||
                               solve(pos + k + 1, k + 1);
      return dp[{pos, k}] = false;
   }
   bool canCross(vector<int>& stones) {
      lastPos = stones.back();
      for (int x : stones)
         stone[x] = 1;
      if (stones[1] == 1)
         return solve(1, 1);
      return false;
   }
};
```

## 👻 About

이전에 풀었던 문제에서 visited배열에서 이전 방문하는 값이 1인지 2인지에 따라 다르게 구별하던 문제가 있었는데, 이 문제의 확장판이라고 보면 됩니다!👍👍👍👍

## ✈ Explain Code

```java
import java.util.*;

class Solution {
    public int size;
    public HashMap<Integer, Boolean>[] dp;
    public boolean solve(int pos, int k, int[] stones){
        if(pos == size - 1){
            return true; //도착하면 true
        }
        HashMap<Integer, Boolean> map = dp[pos];
        // 해당 위치에 k에 따른 value값을 확인
        if(!map.isEmpty() && map.containsKey(k)){
            return map.get(k);
        } // 해당 k값에 까라 존재할 경우
        for(int i = pos + 1; i < size; i++){ // post에서 다음 위치까지 갈 수 있는 위치를 확인합니다.
            int distance = stones[i] - stones[pos];
            if(distance < k - 1){
                continue;
            } else if(distance > k + 1){
                break;
            } else {
                if(solve(i, distance, stones)){
                    map.put(k, true);
                    return true; //가능하면 true
                }
            } // k -1, k, k + 1만 가능
        }
        map.put(k, false);
        return false; // 불가능하면 false
    }
    public boolean canCross(int[] stones) {
        size = stones.length;
        dp = new HashMap[size];
        for(int i = 0; i < size; i++){
            dp[i] = new HashMap<Integer, Boolean>();
        } // init
        if(stones[1] == 1){
            return solve(1, 1, stones);
        } // 첫번째 k는 1입니다!
        return false;
    }
}
```

## ✈ Source Code

```java
import java.util.*;

class Solution {
    public int size;
    public HashMap<Integer, Boolean>[] dp;
    public boolean solve(int pos, int k, int[] stones){
        if(pos == size - 1){
            return true;
        }
        HashMap<Integer, Boolean> map = dp[pos];
        if(!map.isEmpty() && map.containsKey(k)){
            return map.get(k);
        }
        for(int i = pos + 1; i < size; i++){
            int distance = stones[i] - stones[pos];
            if(distance < k - 1){
                continue;
            } else if(distance > k + 1){
                break;
            } else {
                if(solve(i, distance, stones)){
                    map.put(k, true);
                    return true;
                }
            }
        }
        map.put(k, false);
        return false;
    }
    public boolean canCross(int[] stones) {
        size = stones.length;
        dp = new HashMap[size];
        for(int i = 0; i < size; i++){
            dp[i] = new HashMap<Integer, Boolean>();
        }
        if(stones[1] == 1){
            return solve(1, 1, stones);
        }
        return false;
    }
}
```

## ✔ 회고

.
