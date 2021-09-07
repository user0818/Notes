# 题目描述	2021年4月23日20:42:07

https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/

> 输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。
>
> 例如：
>
> 给定二叉树 [3,9,20,null,null,15,7]，
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回它的最大深度 3 。

## 题解

### 解法一：递归法

本题是典型的递归算法的入门题目。

对于任意一个节点来说，它的深度为左右子树深度最大值+1。

递归边界：指针为null，返回0

递归问题：求左子树和右子树的深度，取二者的最大值+1

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
    public int maxDepth(TreeNode root) {
        if(root==null)  return 0;
        if(root.left==null && root.right==null)
            return 1;

        int left=maxDepth(root.left);
        int right=maxDepth(root.right);
        return Math.max(left,right)+1;
    }
}
```

