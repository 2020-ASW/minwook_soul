# ☎ Letter Combinations of a Phone number

## :memo:문제 설명

1. Phone number마다 지정된 문자열이 있습니다.
2. digits에 따라 번호를 누르게 되는데, 누르는 순서에 따라 조합할 수 있는 문자열을 answer배열에 저장합니다.

## 💪풀이 방법

1. Phone number마다 지정된 문자열을 만듭니다.
2. digits에 따라 누르는 순서를 재귀로 반복하여 나온 결과값을 answer배열에 추가해줍니다.

### 👻접근 방식 ans 👻About

1. 우선, Phone number마다 지정된 문자열을 만듭니다.

결과값이 `[[a, b, c], [d, e, f], [g, h, i], [j, k, l], [m, n, o], [p, q, r, s], [t, u, v], [w, x, y, z]]`가 나온다면 어떠한 방식을 사용해도 무관합니다.

```java
public void getMap(){
    ArrayList<Character> element = new ArrayList<Character>();
    ArrayList<Integer> number = new ArrayList<Integer>(Arrays.asList(3, 6, 9, 12, 15, 19, 22, 26));
    for(int i = 1; i <= 26; i++){
        char word = (char)('a' + i - 1);
        element.add(word);
        if(number.contains(i)){
            Map.add(element);
            element = new ArrayList<Character>();
        }
    }
}
```

2. digits에 따라 누르는 순서를 재귀로 반복하는 함수를 구현합니다.

count를 점차 늘려가는 방식으로 사용하여 str에 해당되는 다이얼 문자를 추가해줍니다.
`digits.charAt(count) - '0' - 2`는 ['a','b','c']가 해당되는 숫자가 2이므로 Map에 해당되는 문자열을 가져오는데 사용됩니다.  
▶ Map = `[[a, b, c], [d, e, f], [g, h, i], [j, k, l], [m, n, o], [p, q, r, s], [t, u, v], [w, x, y, z]]`

```java
public void Count(String str, String digits, int count){
    if(count == size){
        answer.add(str);
        return;
    }
    for(Character i : Map.get(digits.charAt(count) - '0' - 2)){
        Count(str + i, digits, count + 1);
    }
}
```

3. count가 digits.length()와 같으면 answer에 추가해주고 return해줍니다.

## Source Code

```java
import java.util.ArrayList;

class Solution {
    public ArrayList<ArrayList<Character>> Map;
    public ArrayList<String> answer;
    public int size;
    public void getMap(){
        ArrayList<Character> element = new ArrayList<Character>();
        ArrayList<Integer> number = new ArrayList<Integer>(Arrays.asList(3, 6, 9, 12, 15, 19, 22, 26));
        for(int i = 1; i <= 26; i++){
            char word = (char)('a' + i - 1);
            element.add(word);
            if(number.contains(i)){
                Map.add(element);
                element = new ArrayList<Character>();
            }
        }
    }
    public void Count(String str, String digits, int count){
        if(count == size){
            answer.add(str);
            return;
        }
        for(Character i : Map.get(digits.charAt(count) - '0' - 2)){
            Count(str + i, digits, count + 1);
        }
    }
    public List<String> letterCombinations(String digits) {
        answer = new ArrayList<String>();
        Map = new ArrayList<ArrayList<Character>>();
        getMap();
        size = digits.length();
        if(size == 0){
            return answer;
        } else {
            Count("", digits, 0);
        }
        return answer;
    }
}
```

### ✔ 회고

`[[a, b, c], [d, e, f], [g, h, i], [j, k, l], [m, n, o], [p, q, r, s], [t, u, v], [w, x, y, z]]`은 항상 똑같으므로 굳이 코드로 짜는 것보다 그냥 입력해도 무방하다고 생각했습니다.
