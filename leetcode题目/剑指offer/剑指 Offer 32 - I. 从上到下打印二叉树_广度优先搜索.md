# 题目描述	2021年5月20日19:46:29

https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/

> 从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。
>
> 例如:
>
> 给定二叉树: [3,9,20,null,null,15,7],
>
> ```
> 	3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回：
>
> [3,9,20,15,7]
>
>
> 提示：
>
> 节点总数 <= 1000
>

# 题解

## 解法一：层序遍历

没什么好说的。

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
    public int[] levelOrder(TreeNode root) {
        if(root==null) return new int[0];
        List<Integer> list=new LinkedList<>();
        Queue<TreeNode> queue=new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()){
            TreeNode cur=queue.poll();
            list.add(cur.val);
            if(cur.left!=null)queue.offer(cur.left);
            if(cur.right!=null)queue.offer(cur.right);
        }
        
        int[] res=new int[list.size()];
        for(int i=0;i<list.size();++i){
            res[i]=list.get(i);
        }
        return res;
    }
}
```

