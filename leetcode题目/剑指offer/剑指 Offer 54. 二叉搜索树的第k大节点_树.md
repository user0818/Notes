## 题目描述	2021年4月25日21:18:06

https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/

> 
> 给定一棵二叉搜索树，请找出其中第k大的节点。
>
>  
>
> **示例 1:**
>
> ```
> 输入: root = [3,1,4,null,2], k = 1
>    3
>   / \
>  1   4
>   \
>    2
> 输出: 4
> ```
>
> **示例 2:**
>
> ```
> 输入: root = [5,3,6,2,4,null,null,1], k = 3
>        5
>       / \
>      3   6
>     / \
>    2   4
>   /
>  1
> 输出: 4
> ```
>
>  
>
> **限制：**
>
> 1 ≤ k ≤ 二叉搜索树元素个数

## 题解

### 解法一：

因为题目给的树是二叉搜索树，因此对该树进行中序遍历得到的结果即为递增的顺序。把结果存在一个列表中，倒数第k个即是第k大的数。

为了更直观显示，又定义了一个新的递归函数执行中序遍历。

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
    public int kthLargest(TreeNode root, int k) {
        ArrayList<Integer> list=new ArrayList<Integer>();
        recursion(root,list);
        return list.get(list.size()-k);
    }
    //中序遍历搜索树
    public void recursion(TreeNode root,ArrayList list){
        if(root==null) return;
        recursion(root.left,list);
        list.add(root.val);
        recursion(root.right,list);
    }
}
```





























