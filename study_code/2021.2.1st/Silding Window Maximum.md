# 📕 Solution

deque를 이용할 것!!

```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int nums_size = nums.length, count = 0;
        int[] result = new int[nums_size - k + 1];
        Deque<Integer> deque = new LinkedList<Integer>();
        for(int i = 0; i < k; i++){
            while(!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(i);
        }
        result[count++] = nums[deque.peek()];
        for(int i = k; i < nums_size; i++){
            while(!deque.isEmpty() && deque.peek() <= i - k){
                deque.removeFirst();
            }
            while(!deque.isEmpty() && nums[i] >= nums[deque.peekLast()]){
                deque.removeLast();
            }
            deque.addLast(i);
            result[count++] = nums[deque.peek()];
        }
        return result;
    }
}
```

## ❌ fail

문제점 : 시간초과

```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int nums_size = nums.length, count = 0;
        ArrayList<Integer> answer = new ArrayList<Integer>();
        for(int i = 0; i < nums_size - k + 1; i++){
            int num = Integer.MIN_VALUE;
            for(int j = i; j < i + k; j++){
                if(num < nums[j]){
                    num = nums[j];
                }
            }
            answer.add(num);
        }
        int[] result = new int[answer.size()];
        for(Integer i : answer){
            result[count++] = i;
        }
        return result;
    }
}
```

```java
import java.util.*;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int nums_size = nums.length, count = 0;
        int[] result = new int[nums_size - k + 1];
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>((o1, o2) -> o2 - o1);
        for(int i = 0; i < k; i++){
            maxHeap.add(nums[i]);
        }
        result[count++] = maxHeap.peek();
        maxHeap.remove(nums[0]);
        for(int i = k; i < nums_size; i++){
            maxHeap.add(nums[i]);
            result[count++] = maxHeap.peek();
            maxHeap.remove(nums[i - k + 1]);
        }
        return result;
    }
}
```
