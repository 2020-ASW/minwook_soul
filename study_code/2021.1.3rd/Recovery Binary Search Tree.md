```java
class Solution {
    TreeNode first, second, prev;
    public void OrderTree(TreeNode root){
        if(root == null) return;
        OrderTree(root.left);
        if(prev == null){
            prev = root;
        } else {
            if(root.val < prev.val){
                if(first == null) first = prev;
                second = root;
            }
            prev = root;
        }
        OrderTree(root.right);
    }
    public void recoverTree(TreeNode root) {
        if(root == null) return;
        OrderTree(root);
        if(first != null && second != null){
            int temp = first.val;
            first.val = second.val;
            second.val = temp;
        }
    }
}
```
