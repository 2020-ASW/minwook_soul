# 📕 Solution

```java
import java.util.*;

class Solution {
    public int[] result, List;
    public void BinarySearch(int NUM, List<List<Integer>> nums){
        int start = 0, end = NUM, list_size = nums.size(), Last = List[List.length - 1], range_size = Integer.MAX_VALUE;
        while(start <= end){
            int mid = (start + end) / 2;
            boolean isComplete = false;
            for(Integer k : List){
                int count = 0;
                //System.out.println("D " + k + " " + (k + mid));
                for(int i = 0; i < list_size; i++){
                    int num = 0;
                    for(Integer j : nums.get(i)){
                        if(j < k){
                            continue;
                        } else if(j > k + mid){
                            break;
                        } else {
                            num++;
                        }
                    }
                    if(num == 0){
                        count = -1;
                        break;
                    }
                    count += num;
                }
                if(count != -1){
                    isComplete = true;
                    if(mid < range_size){
                        range_size = mid;
                        result[0] = k;
                        result[1] = k + mid;
                        //System.out.println("A " + result[0] + " " + result[1] + " " + mid);
                    }
                }
            }
            if(isComplete){
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
    }
    public int[] smallestRange(List<List<Integer>> nums) {
        int list_size = nums.size(), Min = Integer.MAX_VALUE, Max = Integer.MIN_VALUE;
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i = 0; i < list_size; i++){
            Min = Math.min(Min, nums.get(i).get(0));
            Max = Math.max(Max, nums.get(i).get(nums.get(i).size() - 1));
            for(Integer j : nums.get(i)){
                set.add(j);
            }
        }
        int count = 0;
        List = new int[set.size()];
        for(Integer i : set){
            List[count++] = i;
        }
        result = new int[2];
        result[0] = Min;
        result[1] = Max;
        BinarySearch(Max - Min, nums);
        return result;
    }
}
```

## ❌ fail

.
