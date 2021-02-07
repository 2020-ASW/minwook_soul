# 📕 Solution

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;

class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine());
        StringBuffer sb = new StringBuffer();
        long N = Long.parseLong(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        int Q = Integer.parseInt(st.nextToken());
        for (int i = 0; i < Q; i++) {
            st = new StringTokenizer(br.readLine());
            long a = Long.parseLong(st.nextToken());
            long b = Long.parseLong(st.nextToken());
            long answer = 0;
            if (K == 1) {
                answer = (a < b) ? b - a : a - b;
                sb.append(answer + "\n");
                continue;
            }
            while (a != b) {
                if (a < b) {
                    b = (b - 2) / K + 1;
                } else if (a > b) {
                    a = (a - 2) / K + 1;
                } else {
                    break;
                }
                answer++;
            }
            sb.append(answer + "\n");
        }
        bw.write(sb.toString());
        bw.close();
        br.close();
    }
}
```

## ❌ fail

N은 Long이고 절대 메모리를 할당해서 풀면 안됩니다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.ArrayList;
import java.util.StringTokenizer;
import java.util.Arrays;

class Main {
    public static int N, K, Q;
    public static int[] graph, distance;

    public static int Distance(int a, int b) {
        distance[a] = 0;
        distance[b] = 0;
        if (a == b)
            return 0;
        while (true) {
            if (a != 1 && distance[graph[a]] != -1) {
                return distance[graph[a]] + distance[a] + 1;
            }
            if (a != 1 || distance[graph[a]] == -1) {
                distance[graph[a]] = distance[a] + 1;
                a = graph[a];
            }
            if (b != 1 && distance[graph[b]] != -1) {
                return distance[graph[b]] + distance[b] + 1;
            }
            if (b != 1 || distance[graph[b]] == -1) {
                distance[graph[b]] = distance[b] + 1;
                b = graph[b];
            }
        }
    }

    public static void Print() {
        for (int i = 1; i <= N; i++) {
            System.out.print(distance[i] + " ");
        }
        System.out.print("\n");
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        StringBuffer sb = new StringBuffer();
        N = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());
        Q = Integer.parseInt(st.nextToken());
        graph = new int[N + 1];
        int count = 2;
        for (int i = 1; i <= N; i++) {
            for (int j = 0; j < K; j++) {
                graph[count++] = i;
                if (count > N)
                    break;
            }
            if (count > N)
                break;
        }
        distance = new int[N + 1];
        for (int i = 0; i < Q; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            Arrays.fill(distance, -1);
            sb.append(Distance(a, b) + "\n");
        }
        bw.write(sb.toString());
        bw.close();
        br.close();
    }
}
```
