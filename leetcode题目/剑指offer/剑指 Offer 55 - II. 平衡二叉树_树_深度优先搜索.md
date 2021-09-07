## 题目描述	2021年4月27日00:26:53

https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/

> 输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。
>
>  
>
> 示例 1:
>
> 给定二叉树 [3,9,20,null,null,15,7]
>
> ```
> 	3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回 true 。
>
> 示例 2:
>
> 给定二叉树 [1,2,2,3,3,null,null,4,4]
>
> ```
>    1
>   / \
>  2   2
> / \
> 
>    3   3
>   / \
>  4   4
> ```
>
> 返回 false 。
>
>  
>
> 限制：
>
> 0 <= 树的结点个数 <= 10000
>

## 题解

### 解法一

平衡二叉树即树中所有节点左右子树深度相差不大于一。

因此抽取出一个函数来实现求二叉树的深度。

那么主函数就可以借此求出左子树的深度和右子树的深度，算其差值。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root==null)
            return true;
        if(isBalanced(root.left) && isBalanced(root.right)){
            int leftDepth=getDepth(root.left);
            int rightDepth=getDepth(root.right);

            if(Math.abs(leftDepth-rightDepth)<=1)
                return true;                
        }
        return false;
    }
    public int getDepth(TreeNode root){
        if(root==null)
            return 0;
        return Math.max(getDepth(root.left),getDepth(root.right))+1;
    }
}
```

