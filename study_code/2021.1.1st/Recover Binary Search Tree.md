# 📕 Solution

참고 사이트 : [Go to link](https://www.programcreek.com/2014/05/leetcode-recover-binary-search-tree-java/)

```java
class Solution {
    TreeNode first, second, prev;

    public void OrderedTree(TreeNode root){
        if(root == null){
            return;
        }
        OrderedTree(root.left);
        if(prev == null){
            prev = root;
        } else {
            if(root.val < prev.val){
                if(first == null){
                    first = prev;
                }
                second = root;
            }
            prev = root;
        }
        OrderedTree(root.right);
    }

    public void recoverTree(TreeNode root) {
        if(root == null){
            return;
        }
        OrderedTree(root);
        if(first != null && second != null){
            int val = second.val;
            second.val = first.val;
            first.val = val;
        }
    }
}
```
