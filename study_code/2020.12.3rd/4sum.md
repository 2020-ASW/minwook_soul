```java
import java.util.ArrayList;
import java.util.Arrays;

class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        List<List<Integer>> answer = new ArrayList<List<Integer>>();
        int size = nums.length, sum, start, end;
        Arrays.sort(nums);
        for(int a = 0; a < size - 3; a++){
            for(int b = a + 1; b < size - 2; b++){
                sum = nums[a] + nums[b];
                start = b + 1;
                end = size - 1;
                while(start < end){
                    if(sum + nums[start] + nums[end] < target){
                        start +=1;
                    } else if(sum + nums[start] + nums[end] > target){
                        end -= 1;
                    } else {
                        List<Integer> element = new ArrayList<Integer>(Arrays.asList(nums[a],nums[b], nums[start], nums[end]));
                        if(!answer.contains(element)){
                            answer.add(element);
                        }
                        start += 1;
                        end -= 1;
                    }
                }
            }
        }
        return answer;
    }
}
```
