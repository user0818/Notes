# 题目描述	2021年4月23日21:11:41

https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

镜像输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

 

示例 1：

输入：root = [4,2,7,1,3,6,9]

输出：[4,7,2,9,6,3,1]


限制：

0 <= 节点个数 <= 1000

## 题解

### 解法一：递归法

递归入门题。

思路：

把每个结点的左右儿子交换位置。

递归出口：指针为null，返回null

递归问题：交换左右儿子，然后对左右儿子进行递归。

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
    public TreeNode mirrorTree(TreeNode root) {
        if(root==null)
            return null;
        TreeNode tmp=root.left;
        root.left=root.right;
        root.right=tmp;
        mirrorTree(root.left);
        mirrorTree(root.right);
        return root;
    }
}
```

