## 题目描述	2021年4月26日13:51:14

https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
> 例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
>
> 
>
>  
>
> **示例 1:**
>
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
>
> 输出: 3
>
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
>
> **示例 2:**
>
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
>
> 输出: 5
>
> 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
>
> **说明:**
>
> 所有节点的值都是唯一的。
>
> p、q 为不同节点且均存在于给定的二叉树中。

## 题解

### 解法一

1. 既然是公共祖先，那么以这个祖先结点为根节点的子树中必定包含这两个目标结点。

2. 接下来要考虑==最近==这一特点。因为如果某一个结点是他俩的公共祖先，那么这一结点的父节点也是他们的公共祖先。所以这个最近要求的是所有满足要求的结点中最深的那个。

因此我们把这个题分成两部分：1.判断以某一节点为根节点的树，是否同时包含另两个结点。2.找到最深的结点，以该节点为根节点的子树包含指定的两个结点

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
    //如果一个树的左右子树都不能分别同时包含这两个节点，那么就返回根节点
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
            if(haveDoubleNode(root.left,p,q))
                return lowestCommonAncestor(root.left,p,q);
            else if(haveDoubleNode(root.right,p,q))
                return lowestCommonAncestor(root.right,p,q);
            else return root;
        
    }
    //以某一节点为根节点的树，是否同时包含另两个结点
    public boolean haveDoubleNode(TreeNode root,TreeNode p,TreeNode q){
        return haveNode(root,p)&&haveNode(root,q);
    }
    //以某一节点为根节点的树，是否包含另一个结点
    public boolean haveNode(TreeNode root,TreeNode p){
        if(root==null)
            return false;
        if(root.val==p.val){
            return true;
        }
        return haveNode(root.left,p)||haveNode(root.right,p);
    }
        
}
```

