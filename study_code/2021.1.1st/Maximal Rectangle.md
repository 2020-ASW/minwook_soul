# 📕 Solution

## ✔ O(N^4)로 하는 방법

직사각형에 대한 가로의 시작 index, 끝 index, 세로의 시작 index, 끝 index를 비교해서 만듭니다.

▶ Complie : 시간 초과

## ✔ O(N^3)로 하는 방법

**height[i][j] = matrix[i][j]부터 위에(같은 열) 존재하는 연속된 1의 개수**

열에 대한 계산이 필요 없어지므로 O(N^3)만에 구할 수 있습니다.

Ex)
matrix가 아래와 같다고 하면,  
1 0 1 0 0 0  
1 0 1 1 1 0  
1 1 1 1 1 0  
1 0 0 0 0 0

height가 아래와 같다고 하면,  
1 0 1 0 0 0  
2 0 2 1 1 0  
3 1 3 2 2 0  
4 0 0 0 0 0  
로 가정해서 계산

▶ Complie : 시간 초과

## ✔ O(N^2)로 하는 방법

이 방법은 `Maximum Rectangular Area in Histogram`에 나오는 유명한 알고리즘이고 stack을 이용합니다.

```
1) Create an empty stack.

2) Start from first bar, and do following for every bar ‘hist[i]’ where ‘i’ varies from 0 to n-1.
……a) If stack is empty or hist[i] is higher than the bar at top of stack, then push ‘i’ to stack.
……b) If this bar is smaller than the top of stack, then keep removing the top of stack while top of the stack is greater. Let the removed bar be hist[tp]. Calculate area of rectangle with hist[tp] as smallest bar. For hist[tp], the ‘left index’ is previous (previous to tp) item in stack and ‘right index’ is ‘i’ (current index).

3) If the stack is not empty, then one by one remove all bars from stack and do step 2.b for every removed bar.
```

참고 사이트 : [Go to link](https://www.geeksforgeeks.org/largest-rectangle-under-histogram/)

번역문:

```
1. 빈 stack을 만듭니다.

2. 첫 번째 bar에서 시작하여 모든 bar hist[i]에 대해 다음 작업을 수행합니다. 여기서 'i'는 0부터 n-1까지 다양합니다.
__a)__ stack이 비어 있거나 hist[i]가 stack 맨 위에 있는 값보다 높으면 i를 push합니다.
__b)__ 이 bar가 stack의 맨 위보다 낮으면 stack의 맨 위의 값이 낮아질 때까지 stack의 맨 위를 계속 제거합니다. hist[tp]로 값을 가지고 있는 bar를 제거합니다. hist[tp]를 가장 작은 bar로 하여 직사각형의 면적을 계산합니다. hist[tp]의 경우 '왼쪽 인덱스'는 stack의 tp 이전 항목이고 '오른쪽 인덱스'는 'i'(현재 인덱스)입니다. 면적은 stack이 비어있으면 hist[tp] * i, 아니면 hist[tp] * (i - stack.peek()(=stack의 최상단 값) - 1) 입니다.

* 해당 면적 알고리즘:
if(stack.isEmpty()){
    area = hist[tp] * i;
} else {
    area = hist[tp] * (i - stack.peek() - 1);
}

3. 스택이 비어 있지 않은 경우 스택에서 모든 막대를 하나씩 제거하고 제거된 막대에 대해 2.b 단계를 수행합니다.
```

설명 추가 :

height를 row별로 보게되면 (`int[] height = new int[y_size + 1];`)

i = 0인 경우, 1 0 1 0 0 0  
i = 1인 경우, 2 0 2 1 1 0  
i = 2인 경우, 3 1 3 2 2 0  
i = 3인 경우, 4 0 0 0 0 0

각 row별로 가장 큰 히스토그램 면적을 구합니다. 그 값을 Max에 저장합니다.

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0) return 0;
        // matrix에 value가 없는 경우를 return
        int x_size = matrix.length, y_size = matrix[0].length, Max = 0, area = 0;
        int[] height = new int[y_size + 1];
        for(int i = 0; i < x_size; i++){
            Stack<Integer> stack = new Stack<Integer>();
            for(int j = 0; j <= y_size; j++){
                if(j == y_size || matrix[i][j] == '0'){
                    height[j] = 0;
                } else {
                    height[j]++;
                }
                // 2.(b)에 해당
                while(!stack.isEmpty() && height[stack.peek()] >= height[j]){
                    int Height = height[stack.pop()];
                    if(stack.isEmpty()){
                        area = Height * j;
                    } else {
                        area = Height * (j - stack.peek() - 1);
                    }
                    if(Max < area) Max = area; // area의 최댓값 저장
                }
                // 2.(a)에 해당
                stack.push(j);
            }
        }
        return Max;
    }
}
```

코드 원문 :

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        if(matrix.length == 0 || matrix[0].length == 0) return 0;
        int x_size = matrix.length, y_size = matrix[0].length, Max = 0, area = 0;
        int[] height = new int[y_size + 1];
        for(int i = 0; i < x_size; i++){
            Stack<Integer> stack = new Stack<Integer>();
            for(int j = 0; j <= y_size; j++){
                if(j == y_size || matrix[i][j] == '0'){
                    height[j] = 0;
                } else {
                    height[j]++;
                }
                while(!stack.isEmpty() && height[stack.peek()] >= height[j]){
                    int Height = height[stack.pop()];
                    if(stack.isEmpty()){
                        area = Height * j;
                    } else {
                        area = Height * (j - stack.peek() - 1);
                    }
                    if(Max < area) Max = area;
                }
                stack.push(j);
            }
        }
        return Max;
    }
}
```

## ⭐ 참고 사이트 :

dp로 구현한 Java 코드 : [Go to link](https://github.com/cherryljr/LeetCode/blob/master/Maximal%20Rectangle.java)  
Maximum Rectangular Area in Histogram 설명 사이트 : [Go to link](https://www.programcreek.com/2014/05/leetcode-largest-rectangle-in-histogram-java/)
