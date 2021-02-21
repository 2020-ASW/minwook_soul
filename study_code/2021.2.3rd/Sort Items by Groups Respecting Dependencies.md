# 📕 Solution

## :memo: 문제 설명

1. n개의 item과 m개의 group이 존재합니다. 만약 group이 -1인 경우는 아무 그룹에도 속하지 않습니다.
2. 해당 item에 따른 Before배열은 비어있거나 여러 개의 item으로 구성되어져 있습니다. 해당 Before배열에 있는 원소들은 item보다 배열 앞에 존재해야만 합니다.
3. 해당 조건을 만족하는 item 작업 순서를 반환합니다.

## 👻 접근 방식

1. 해당 item들을 group끼리 묶어줍니다. item 수만큼의 itemGraph를, group 수만큼의 groupGraph를 선언해줍니다.
2. group이 같은 item들끼리 작업 순서가 정해져 있다면(=before배열에 속해 있다면) itemGraph에 간선을 연결해줍니다.
3. group이 다른 item들끼리 작업 순서가 정해져 있다면(=before배열에 속해 있다면) itemGraph에 간선을 연결해준 다음, 해당 group에 따른 작업 순서도 간선으로 연결해줍니다.
4. Topological Sorting를 이용해서 item 작업순서와 group 작업순서를 정해줍니다.
5. item 작업 순서에 맞게 group을 묶은 다음, group 작업 순서에 맞게 정렬해줍니다.
6. 정렬된 int배열을 반환합니다.

## ❌ fail

### 문제점 : Wrong Answer

**group이 -1인 경우는 아무 그룹에도 속하지 않습니다.**  
하지만 Group에 해당되는 숫자로 grouping을 해서 -1인 그룹이 만들어졌습니다.  
그러므로 -1인 item들을 분리해야 합니다.

```java
import java.util.*;

class Graph {
    int n;
    ArrayList<ArrayList<Integer>> next;
    Graph(int n){
        this.n = n;
        this.next = new ArrayList<>();
        for(int i = 0; i < n; i++){
            next.add(new ArrayList<>());
        }
    }
    List<Integer> sort(){
        // Kahn's algorithm for Topological Sorting를 이용해서 순서를 정해줍니다.
        int[] rank = new int[n];
        for(int i = 0; i < n; i++){
            for(Integer j : next.get(i)){
                rank[j] += 1; // 들어오는 인접 node 갯수
            }
        }
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i = 0; i < n; i++){
            if(rank[i] == 0){
                queue.offer(i);
            }
        }
        int count = 0;
        ArrayList<Integer> answer = new ArrayList<Integer>();
        while(!queue.isEmpty()){
            int node = queue.poll();
            answer.add(node);
            count++;
            for(Integer e : next.get(node)){
                rank[e] -= 1;
                if(rank[e] == 0){
                    queue.offer(e);
                }
            }
        }
        if(count < n) {
            return new ArrayList<>(); // 모든 node에 도달 못하는 경우
        }
        return answer;
    }
}

class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        Graph itemGraph = new Graph(n);
        Graph groupGraph = new Graph(m+1);
        //init
        // groupGraph에서 -1을 index 0으로 확인할 예정
        for(int i = 0; i < n; i++){
            for(Integer e : beforeItems.get(i)){
                itemGraph.next.get(e).add(i); // e -> i 가 나와야 합니다.
                if(group[i] != group[e]){
                    groupGraph.next.get(group[e] + 1).add(group[i] + 1);
                } // e group -> i group 순으로 나와야 합니다.
            }
        }
        List<Integer> itemOrder = itemGraph.sort();
        List<Integer> groupOrder = groupGraph.sort();
        /*
        for(Integer i : itemOrder){
            System.out.print(i + " ");
        }
        System.out.print("\n");
        for(Integer i : groupOrder){
            System.out.print(i + " ");
        }
        System.out.print("\n");
        */
        if(itemOrder.isEmpty() || groupOrder.isEmpty()){
            return new int[0];
        } // cycle이 존재하는 경우

        HashMap<Integer, List<Integer>> intraOrder = new HashMap<>();
        int itemOrder_size = itemOrder.size(), groupOrder_size = groupOrder.size(), k = 0;
        for(int i = 0; i < itemOrder_size; i++){
            int num = itemOrder.get(i);
            int group_num = group[num] + 1;
            intraOrder.putIfAbsent(group_num, new ArrayList<>()); // group_num인 key로 된 원소가 없으면 해당 원소 추가
            intraOrder.get(group_num).add(num);
        }
        int[] answer = new int[n];
        for(int i = 0; i < groupOrder_size; i++){
            int x = groupOrder.get(i);
            if(intraOrder.containsKey(x)){
                for(Integer e : intraOrder.get(x)){
                    answer[k++] = e;
                }
            }
        }
        return answer;
    }
}
```

## 💪 풀이 방법

해당 item에 대한 Group number가 -1이라면, 해당 번호를 서로 겹치지 않는 다른 번호로 바꿔줍니다.

```java
for(int i = 0; i < m; i++){
    groupGraph.next.add(new ArrayList<>());
}
for(int i = 0; i < group.length; i++){
    if(group[i] == -1){
        group[i] = m++;
        groupGraph.next.add(new ArrayList<>());
    }
}
// init
```

## 👻 About

Graph class에 있는 sort function에 Topological Sorting을 사용하였습니다.  
Topological Sorting를 하는 방식은 이전에 풀었던 "작업 순서"라는 문제를 참고하시면 됩니다.

**1월 1주차 작업 순서 참고 :** [Go to Link](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV18TrIqIwUCFAZN&categoryId=AV18TrIqIwUCFAZN&categoryType=CODE)

## ✈ Explain Code

```java
import java.util.*;

class Graph {
    int n;
    ArrayList<ArrayList<Integer>> next;
    Graph(int n){
        this.n = n;
        this.next = new ArrayList<>();
    }
    List<Integer> sort(int n){
        // Kahn's algorithm for Topological Sorting를 이용해서 순서를 정해줍니다.
        int[] rank = new int[n];
        for(int i = 0; i < n; i++){
            for(Integer j : next.get(i)){
                rank[j] += 1; // 들어오는 인접 node 갯수
            }
        }
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i = 0; i < n; i++){
            if(rank[i] == 0){
                queue.offer(i);
            }
        }
        int count = 0;
        ArrayList<Integer> answer = new ArrayList<Integer>();
        while(!queue.isEmpty()){
            int node = queue.poll();
            answer.add(node);
            count++;
            for(Integer e : next.get(node)){
                rank[e] -= 1;
                if(rank[e] == 0){
                    queue.offer(e);
                }
            }
        }
        if(count < n) {
            return new ArrayList<>(); // 모든 node에 도달 못하는 경우
        }
        return answer;
    }
}

class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        Graph itemGraph = new Graph(n);
        Graph groupGraph = new Graph(m);
        for(int i = 0; i < n; i++){
            itemGraph.next.add(new ArrayList<>());
        }
        for(int i = 0; i < m; i++){
            groupGraph.next.add(new ArrayList<>());
        }
        for(int i = 0; i < group.length; i++){
            if(group[i] == -1){
                group[i] = m++;
                groupGraph.next.add(new ArrayList<>());
            }
        }
        // init

        for(int i = 0; i < n; i++){ // groupGraph에서 -1은 따로 그룹핑을 해주는 방식으로 만듭니다.
            for(Integer e : beforeItems.get(i)){
                itemGraph.next.get(e).add(i); // e -> i 가 나와야 합니다.
                //System.out.println(i + " " + e +  " " + group[i] +  " " + group[e]);
                if(group[i] != group[e]){
                    groupGraph.next.get(group[e]).add(group[i]);
                } // e group -> i group 순으로 나와야 합니다.
            }
        }
        /*
        for(int i = 0; i < groupGraph.next.size(); i++){
            System.out.print("A: " + i + " ");
            for(Integer e : groupGraph.next.get(i)){
                System.out.print(e + " ");
            }
            System.out.print("\n");
        }
        */
        List<Integer> itemOrder = itemGraph.sort(n);
        List<Integer> groupOrder = groupGraph.sort(groupGraph.next.size());
        int itemOrder_size = itemOrder.size(), groupOrder_size = groupOrder.size(), k = 0;
        /*
        for(Integer i : itemOrder){
            System.out.print(i + " ");
        }
        System.out.print("\n");
        for(Integer i : groupOrder){
            System.out.print(i + " ");
        }
        System.out.print("\n");
        */
        if(itemOrder.isEmpty() || groupOrder.isEmpty()){
            return new int[0];
        } // cycle이 존재하는 경우

        HashMap<Integer, List<Integer>> intraOrder = new HashMap<>();
        for(int i = 0; i < itemOrder_size; i++){
            int num = itemOrder.get(i);
            int group_num = group[num];
            intraOrder.putIfAbsent(group_num, new ArrayList<>()); // group_num인 key로 된 원소가 없으면 해당 원소 추가
            intraOrder.get(group_num).add(num);
        }
        /*
        for(int i = 0; i < intraOrder.size(); i++){
            System.out.print("[" + i + "] ");
            for(Integer j : intraOrder.get(i)){
                System.out.print(j + " ");
            }
            System.out.print("\n");
        }
        */
        int[] answer = new int[n];
        for(int i = 0; i < groupOrder_size; i++){
            int x = groupOrder.get(i);
            if(intraOrder.containsKey(x)){
                for(Integer e : intraOrder.get(x)){
                    answer[k++] = e;
                }
            }
        }
        return answer;
    }
}
```

## ✈ Source Code

```java
import java.util.*;

class Graph {
    int n;
    ArrayList<ArrayList<Integer>> next;
    Graph(int n){
        this.n = n;
        this.next = new ArrayList<>();
    }
    List<Integer> sort(int n){
        int[] rank = new int[n];
        for(int i = 0; i < n; i++){
            for(Integer j : next.get(i)){
                rank[j] += 1;
            }
        }
        Queue<Integer> queue = new LinkedList<Integer>();
        for(int i = 0; i < n; i++){
            if(rank[i] == 0){
                queue.offer(i);
            }
        }
        int count = 0;
        ArrayList<Integer> answer = new ArrayList<Integer>();
        while(!queue.isEmpty()){
            int node = queue.poll();
            answer.add(node);
            count++;
            for(Integer e : next.get(node)){
                rank[e] -= 1;
                if(rank[e] == 0){
                    queue.offer(e);
                }
            }
        }
        if(count < n) {
            return new ArrayList<>();
        }
        return answer;
    }
}

class Solution {
    public int[] sortItems(int n, int m, int[] group, List<List<Integer>> beforeItems) {
        Graph itemGraph = new Graph(n);
        Graph groupGraph = new Graph(m);
        for(int i = 0; i < n; i++){
            itemGraph.next.add(new ArrayList<>());
        }
        for(int i = 0; i < m; i++){
            groupGraph.next.add(new ArrayList<>());
        }
        for(int i = 0; i < group.length; i++){
            if(group[i] == -1){
                group[i] = m++;
                groupGraph.next.add(new ArrayList<>());
            }
        }
        for(int i = 0; i < n; i++){
            for(Integer e : beforeItems.get(i)){
                itemGraph.next.get(e).add(i);
                if(group[i] != group[e]){
                    groupGraph.next.get(group[e]).add(group[i]);
                }
            }
        }
        List<Integer> itemOrder = itemGraph.sort(n);
        List<Integer> groupOrder = groupGraph.sort(groupGraph.next.size());
        int itemOrder_size = itemOrder.size(), groupOrder_size = groupOrder.size(), k = 0;
        if(itemOrder.isEmpty() || groupOrder.isEmpty()){
            return new int[0];
        }
        HashMap<Integer, List<Integer>> intraOrder = new HashMap<>();
        for(int i = 0; i < itemOrder_size; i++){
            int num = itemOrder.get(i);
            int group_num = group[num];
            intraOrder.putIfAbsent(group_num, new ArrayList<>());
            intraOrder.get(group_num).add(num);
        }
        int[] answer = new int[n];
        for(int i = 0; i < groupOrder_size; i++){
            int x = groupOrder.get(i);
            if(intraOrder.containsKey(x)){
                for(Integer e : intraOrder.get(x)){
                    answer[k++] = e;
                }
            }
        }
        return answer;
    }
}
```

## ✔ 회고

자료, 동영상 전부를 찾으면서 확인해보았지만, 다 틀린 코드여서 아이디어만 참고해서 문제를 풀었습니다.😑😑

**(왜 다들 코드 작성을 완료하고 submit을 안 하는거지?)**
