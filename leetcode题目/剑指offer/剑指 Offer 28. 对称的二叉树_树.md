## 题目描述	2021年4月26日23:11:13

https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/

> 请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。
>
> 例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
> ```
>     1
>    / \
>   2   2
>  / \ / \
> 3  4 4  3
> ```
>
> 但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
>
> ```
>     1
>    / \
>   2   2
>    \   \
>    3    3
> ```
>
>  
>
> **示例 1：**
>
> 输入：root = [1,2,2,3,4,4,3]
>
> 输出：true
>
> **示例 2：**
>
> 输入：root = [1,2,2,null,3,null,3]
>
> 输出：false
>
> **限制：**
>
> 0 <= 节点个数 <= 1000
>

## 题解

### 解法一

对称，很容易想到需要两个指针对左右子树进行跟踪。

从根节点的左右两个儿子开始，每个对称节点要满足：

- 两个结点值相等
- **左节点的右儿子**和**右节点的左儿子**对称

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
    public boolean isSymmetric(TreeNode root) {
        return root==null?true:check(root.left,root.right);
    }
    public boolean check(TreeNode left,TreeNode right){
        if(left==null && right==null)
            return true;
        if(left==null || right==null)
            return false;
        return left.val==right.val && check(left.right,right.left) && check(left.left,right.right);
    }
}
```

