# 📕 Solution

```java
class Solution {
    TreeNode root;
    public int Max;

    public int findMaxSum(TreeNode node, int result){
        if(node == null) return 0;
        int left = findMaxSum(node.left, result);
        int right = findMaxSum(node.right, result);

        int max_one_direct = Math.max(Math.max(left, right) + node.val, node.val);
        int max_both_direct = left + right + node.val;
        int max = Math.max(max_one_direct, max_both_direct);
        Max = Math.max(Max, max);

        return max_one_direct;
    }

    public int maxPathSum(TreeNode root) {
        Max = Integer.MIN_VALUE;
        findMaxSum(root, Max);
        return Max;
    }
}
```

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */

class Result {
    public int max;
} // 여러 재귀 호출에 동시에 접근이 가능하도록 class 선언

class Solution {
    TreeNode root;

    public int findMaxSum(TreeNode node, Result result){
        if(node == null) return 0;
        int left = findMaxSum(node.left, result);
        int right = findMaxSum(node.right, result);

        int max_one_direct = Math.max(Math.max(left, right) + node.val, node.val); // 왼쪽이나 오른쪽 하나에 대한 합을 확인합니다.
        int max_both_direct = left + right + node.val; // 왼쪽와 오른쪽 두 개에 대한 합을 확인합니다.
        int max = Math.max(max_one_direct, max_both_direct); // 두 가지 경우에 대한 최댓값을 구합니다.
        result.max = Math.max(result.max, max); // max 저장

        return max_one_direct;
    }

    public int maxPathSum(TreeNode root) {
        Result result = new Result();
        result.max = Integer.MIN_VALUE;
        findMaxSum(root, result);
        return result.max;
    }
}
```
