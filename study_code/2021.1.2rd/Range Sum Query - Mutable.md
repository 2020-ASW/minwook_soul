우선적으로 1차원 배열로 만들었는데 Segment Tree, Binary Index Tree로도 가능하다고 하니, 시간 남으면 바로 도전하겠습니다.

참고 사이트 : [Go to Link](https://www.programcreek.com/2014/04/leetcode-range-sum-query-mutable-java/)

```java
class NumArray {
    int[] arr;
    public NumArray(int[] nums) {
        int Size = nums.length;
        arr = new int[Size];
        for(int i = 0; i < Size; i++){
            arr[i] = nums[i];
        }
    }

    public void update(int i, int val) {
        arr[i] = val;
    }

    public int sumRange(int i, int j) {
        int sum = 0;
        for(int index = i; index <= j; index++){
            sum += arr[index];
        }
        return sum;
    }
}
```
