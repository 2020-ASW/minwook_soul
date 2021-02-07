# 📕 Solution

```java
import java.util.*;
import java.util.Map.Entry;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        int size = tasks.length, maxCount = 0;
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        for(int i = 0; i < size; i++){
            map.putIfAbsent(tasks[i], 0);
            map.put(tasks[i], map.get(tasks[i]) + 1);
            if(maxCount < map.get(tasks[i])) maxCount = map.get(tasks[i]);
        }
        List<Entry<Character, Integer>> list_entry = new ArrayList<Entry<Character, Integer>>(map.entrySet());
        Collections.sort(list_entry, new Comparator<Entry<Character, Integer>>(){
            @Override
            public int compare(Entry<Character, Integer> o1, Entry<Character, Integer>o2){
                return o2.getValue() - o1.getValue();
            }
        });
        int idleSum = (maxCount - 1) * n;
        for(Entry<Character, Integer> entry : list_entry){
            idleSum -= Math.min(maxCount - 1, entry.getValue());
        }
        idleSum += maxCount - 1;
        return size + Math.max(0, idleSum);
    }
}
```

[Go to Link](https://github.com/fpdjsns/Algorithm/blob/master/leetcode/medium/621.%20Task%20Scheduler.cpp)

## ❌ fail

```java
import java.util.*;
import java.util.Map.Entry;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        int size = tasks.length;
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        for(int i = 0; i < size; i++){
            map.putIfAbsent(tasks[i], 0);
            map.put(tasks[i], map.get(tasks[i]) + 1);
        }
        List<Entry<Character, Integer>> list_entry = new ArrayList<Entry<Character, Integer>>(map.entrySet());
        Collections.sort(list_entry, new Comparator<Entry<Character, Integer>>(){
            @Override
            public int compare(Entry<Character, Integer> o1, Entry<Character, Integer>o2){
                return o2.getValue() - o1.getValue();
            }
        });
        int start = 0, end = 0;
        char[] test_task = new char[10001];
        for(Entry<Character, Integer> entry : list_entry){
            char word = entry.getKey();
            int count = entry.getValue();
            for(int i = 0; i < count; i++){
                test_task[i * (n + 1) + start] = word;
            }
            if(end < (count - 1) * (n + 1) + start){
                end = (count - 1) * (n + 1) + start;
            }
            for(int i = start + 1; i < 10001; i++){
                if(test_task[i] == 0){
                    start = i;
                    break;
                }
            }
        }
        return end + 1;
    }
}
```
