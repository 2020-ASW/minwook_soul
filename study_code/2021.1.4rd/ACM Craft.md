# 📕 Solution

topological sort을 이용해서 만들어야 합니다.  
참고 사이트 : [Go to Link](https://github.com/hotehrud/acmicpc/blob/master/algorithm/sort/1005.java)  
위상 정렬 참고 사이트 : [Go to Link](https://gmlwjd9405.github.io/2018/08/27/algorithm-topological-sort.html)

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

class Main {
    public static int[] time_list;
    public static ArrayList<ArrayList<Integer>> graph;

    public static int bfs(int a, int N, int[] indegree) {
        Queue<Integer> queue = new LinkedList<Integer>();
        int[] weight_list = new int[N + 1];
        for (int i = 1; i <= N; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
                weight_list[i] = time_list[i];
            }
        }
        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (Integer next : graph.get(node)) {
                weight_list[next] = Math.max(weight_list[next], weight_list[node] + time_list[next]);
                indegree[next]--;
                if (indegree[next] == 0) {
                    queue.offer(next);
                }
            }
        }
        return weight_list[a];
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int test_case = 1; test_case <= T; test_case++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int N = Integer.parseInt(st.nextToken());
            int K = Integer.parseInt(st.nextToken());
            graph = new ArrayList<>();
            for (int i = 0; i <= N; i++) {
                graph.add(new ArrayList<Integer>());
            }
            st = new StringTokenizer(br.readLine(), " ");
            time_list = new int[N + 1];
            for (int i = 1; i <= N; i++) {
                time_list[i] = Integer.parseInt(st.nextToken());
            }
            int[] indegree = new int[N + 1];
            for (int i = 1; i <= K; i++) {
                st = new StringTokenizer(br.readLine(), " ");
                int start = Integer.parseInt(st.nextToken());
                int end = Integer.parseInt(st.nextToken());
                graph.get(start).add(end);
                indegree[end]++;
            }
            int Answer = Integer.parseInt(br.readLine());
            System.out.println(bfs(Answer, N, indegree));
        }
        br.close();
    }
}
```

# ❌ Fail

문제점 : 메모리 초과 ▶ reverse_graph까지 만들면 메모리가 너무 많이 생깁니다.

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

class Main {
    public static int Max;
    public static int[] time_list, weight_list;
    public static boolean[] visited;
    public static ArrayList<ArrayList<Integer>> graph, reverse_graph;

    public static void bfs(int a, int N) {
        Queue<Integer> queue = new LinkedList<Integer>();
        visited = new boolean[N + 1];
        weight_list = new int[N + 1];
        queue.offer(a);
        weight_list[a] = time_list[a];
        while (!queue.isEmpty()) {
            int node = queue.poll();
            visited[node] = true;
            for (Integer next : reverse_graph.get(node)) {
                if (!visited[next]) {
                    visited[node] = false;
                }
            }
            if (graph.get(node).isEmpty()) {
                continue;
            }
            for (Integer next : graph.get(node)) {
                if (visited[next]) {
                    continue;
                }
                weight_list[next] = Math.max(weight_list[next], weight_list[node] + time_list[next]);
                queue.offer(next);
            }
        }
        for (Integer i : weight_list) {
            if (i > Max)
                Max = i;
        }
        return;
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        for (int test_case = 1; test_case <= T; test_case++) {
            StringTokenizer st = new StringTokenizer(br.readLine(), " ");
            int N = Integer.parseInt(st.nextToken());
            int K = Integer.parseInt(st.nextToken());
            graph = new ArrayList<>(); // W에서 시작되는 그래프
            reverse_graph = new ArrayList<>();
            for (int i = 0; i <= N; i++) {
                graph.add(new ArrayList<Integer>());
                reverse_graph.add(new ArrayList<Integer>());
            }
            st = new StringTokenizer(br.readLine(), " ");
            time_list = new int[N + 1];
            for (int i = 1; i <= N; i++) {
                time_list[i] = Integer.parseInt(st.nextToken());
            }
            for (int i = 1; i <= K; i++) {
                st = new StringTokenizer(br.readLine(), " ");
                int start = Integer.parseInt(st.nextToken());
                int end = Integer.parseInt(st.nextToken());
                graph.get(end).add(start); // reverse
                reverse_graph.get(start).add(end);
            }
            int Answer = Integer.parseInt(br.readLine());
            Max = 0;
            bfs(Answer, N);
            System.out.println(Max);
        }
        br.close();
    }
}
```
