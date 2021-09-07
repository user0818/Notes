## 题目描述	2021年4月26日14:55:55

https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/

> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
> 例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
>
> ![image-20210426145635065](https://gitee.com/mw515031/image/raw/master/image/image-20210426145635065.png)
>
> **示例 1:**
>
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
>
> 输出: 6 
>
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
>
> **示例 2:**
>
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
>
> 输出: 2
>
> 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
>
> **说明:**
>
> 所有节点的值都是唯一的。
>
> p、q 为不同节点且均存在于给定的二叉搜索树中。

## 题解

### 解法一

这个题由于给定的是搜索树，在寻找节点时就有了方向。

- 若两节点分别落在了根节点两边，或者其中一个节点就是根节点本身，那么根节点即为最近公共祖先
- 若两节点落在根节点同一侧，则递归该侧的儿子节点

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
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        if(p.val>q.val){
            TreeNode tmp=p;
            p=q;
            q=tmp;
        }

        if((p.val<root.val&&root.val<q.val)||(root.val==p.val||root.val==q.val)){
            return root;
        }else if(p.val<root.val&& q.val<root.val ){
            return lowestCommonAncestor(root.left,p,q);
        }else
            return lowestCommonAncestor(root.right,p,q);

    }
}
```

