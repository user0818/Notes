# 题目描述	2021年5月15日11:45:04

https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/

> 输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
>
> 例如，给出
>
> ```
> 前序遍历 preorder = [3,9,20,15,7]
> 
> 中序遍历 inorder = [9,3,15,20,7]
> ```
>
> 返回如下的二叉树：
>
> ```
> 	3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> **限制：**
>
> 0 <= 节点个数 <= 5000
>

# 题解

## 解法一：递归

递归问题提取：建立一棵树

前序遍历的第一个肯定是改树的根节点，在中序遍历中找到这个根节点，那么中序遍历中根节点左边的就是左子树，右边的就是右子树，通过子树的结点个数就能找到前序遍历中子树的序列。

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
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return buildTree(preorder,inorder,0,preorder.length-1,0,inorder.length-1);
    }
    public TreeNode buildTree(int[] preorder, int[] inorder,int preleft,int preright,int inleft,int inright) {
        //递归边界
        if(inleft>inright)
            return null;
        //递归问题
        int root_val=preorder[preleft];
        int root_index_in_inorder=-1;
        //这里可以优化一下，提前建立个哈希表，在这里只需要取就好
        for(int i=inleft;i<=inright;++i){
            if(inorder[i]==root_val){
                root_index_in_inorder=i;
                break;
            }
        }
        //创建本次递归的根节点
        TreeNode root=new TreeNode(preorder[preleft]);
        root.left=buildTree(preorder,inorder,
                            preleft+1,preleft+root_index_in_inorder-inleft,
                            inleft,root_index_in_inorder-1);
        root.right=buildTree(preorder,inorder,
                             preleft+root_index_in_inorder-inleft+1,preright,
                             root_index_in_inorder+1,inright);
        return root;
    }

}
```

