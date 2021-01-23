# 📕 Solution

모든 key를 해당 원소의 빈도수로 만들어줍니다.

```java
import java.util.*;

class FreqStack {
    public HashMap<Integer, Stack<Integer>> stack; // <해당 원소의 빈도 수, 해당 원소>
    public HashMap<Integer, Integer> map; // <해당 원소, 원소의 갯수>
    public int maxfreq;
    public FreqStack() {
        stack = new HashMap<Integer, Stack<Integer>>();
        map = new HashMap<Integer, Integer>();
        maxfreq = 0;
    }

    public void push(int x) {
        int freq = map.getOrDefault(x, 0) + 1;
        map.put(x, freq);
        stack.computeIfAbsent(freq, z -> new Stack()).push(x);
        if(freq > maxfreq) maxfreq = freq;
    }

    public int pop() {
        int top = stack.get(maxfreq).pop(); // 최대 빈도 수가 들어 있는 stack에 pop
        map.put(top, map.get(top) - 1);
        if(stack.get(maxfreq).size() == 0){
            maxfreq -= 1;
        }
        return top;
    }
}

```

# ❌ Fail

문제점 : stack를 arraylist로 만들어서 필요없는 빈도수 작은 원소까지 확인한다.

```java
import java.util.*;

class FreqStack {
    public ArrayList<Integer> stack;
    public HashMap<Integer, Integer> map;
    public FreqStack() {
        stack = new ArrayList<Integer>();
        map = new HashMap<Integer, Integer>();
    }

    public void push(int x) {
        stack.add(x);
        if(map.keySet().contains(x)){
            map.put(x, map.get(x) + 1);
        } else {
            map.put(x, 1);
        }
    }

    public int pop() {
        int Size = stack.size();
        int pop = 0, max = 0;
        for(Integer key : map.keySet()){
            max = Math.max(map.get(key), max);
        }
        for(int i = Size - 1; i >= 0; i--){
            if(map.get(stack.get(i)) == max){
                pop = stack.get(i);
                map.put(pop, map.get(pop) - 1);
                stack.remove(i);
                break;
            }
        }
        return pop;
    }
}
```
