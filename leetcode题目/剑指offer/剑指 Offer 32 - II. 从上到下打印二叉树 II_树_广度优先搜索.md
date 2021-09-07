## 题目描述	2021年4月26日15:21:56

https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/

> 从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
>
>  
>
> **例如:**
>
> 给定二叉树: [3,9,20,null,null,15,7],
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 返回其层次遍历结果：
>
> ```
> [
>   [3],
>   [9,20],
>   [15,7]
> ]
> ```
>
> **提示：**
>
> 节点总数 <= 1000
>

## 题解

### 解法一：广度优先搜索

标准的广度优先搜索

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
    public List<List<Integer>> levelOrder(TreeNode root) {
        //二维列表
        List<List<Integer>> outer=new ArrayList<>();
        //辅助队列
        Queue<TreeNode> queue=new LinkedList<>();
		//特殊值处理
        if(root==null) return outer;
        //根节点入队
        queue.offer(root);
        int count=0;
        //只要辅助队列不为空就一直执行
        while(!queue.isEmpty()){
            //一维列表，存储该层的结果
            List<Integer> inner=new ArrayList<>();
            //遍历"当前"队列，也就是这一层，注意这个i的范围
            for(int i=queue.size();i>0;--i){
                TreeNode cur=queue.poll();
                inner.add(cur.val);
                //把该层每一个结点的子节点入队。
                if(cur.left!=null)queue.offer(cur.left);
                if(cur.right!=null)queue.offer(cur.right);
            }
            //把该层的结果保存到二维列表中
            outer.add(inner);
        }
        return outer;
    }
}
```

