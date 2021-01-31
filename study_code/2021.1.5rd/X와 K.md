# 📕 Solution

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        int X = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        int[] arrX = new int[65];
        for (int i = 0; X > 0; X /= 2) {
            arrX[i++] = X % 2;
        }
        long Y = 0, p = 1;
        for (int i = 0; i < 65 && K > 0; i++, p *= 2) {
            if (arrX[i] == 0) {
                if (K % 2 == 1)
                    Y += p;
                K /= 2;
            }
        }
        System.out.println(Y);
        br.close();
    }
}
```

## ❌ fail

문제점 : 시간초과

```java
import java.io.BufferedReader;
import java.io.BufferedReader;

import java.io.InputStreamReader;
import java.util.StringTokenizer;

class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        long X = Long.parseLong(st.nextToken());
        long K = Long.parseLong(st.nextToken());
        long Y = 0;
        while (K > 0) {
            Y++;
            if (X + Y == (X | Y)) {
                K--;
            }
        }
        System.out.println(Y);
        br.close();
    }
}
```
