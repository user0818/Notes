# 题目描述	2021年5月14日19:13:58

https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/

> 请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。
>
>  
>
> **示例 1：**
>
> 输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
>
> 输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
>
> **示例 2：**
>
> 输入：head = [[1,1],[2,1]]
>
> 输出：[[1,1],[2,1]]
>
> **示例 3：**
>
> 输入：head = [[3,null],[3,0],[3,null]]
>
> 输出：[[3,null],[3,0],[3,null]]
>
> **示例 4：**
>
> 输入：head = []
>
> 输出：[]
>
> 解释：给定的链表为空（空指针），因此返回 null。
>
> **提示：**
>
> -10000 <= Node.val <= 10000
> Node.random 为空（null）或指向链表中的节点。
> 节点数目不超过 1000 。
>
>
> 注意：本题与主站 138 题相同：https://leetcode-cn.com/problems/copy-list-with-random-pointer/

# 题解

常规的链表复制：

- 申请结点
- 构建本节点的值
- 构建前驱节点的next指针
- 构建本节点的next指针

本题的难点在于：

- 申请结点
- 构建本节点的值
- 构建前驱节点的next指针
- 构建本节点的next指针
- ==构建random节点的next指针==

所以本题的难点在于怎么构建random结点。

**我们可不可以在申请结点的过程中构建random结点？**不可以

因为复制是从前往后依次进行的，当你申请完这结点是，你的random结点可能在你后边，还没有申请。

所以我们只能先按常规方法把除了random外的链表的所有内容创建完毕，然后在回过头来构建random结点。

**思路：**

想到这里就很直观了。对于新链表上的某一节点，当我们可以通过原链表上该节点的random指针知道自己指向的结点的值是多少，接下来只需要考虑怎么快速有效的查找到新链表中这个结点即可。**O（N^2）的遍历这里就不提了**

## 解法一：哈希表

我们要通过旧链表的结点找到新链表的结点，如果不想对于每一个节点找的时候时候都遍历一遍，那么们可以采用哈希表，保存原结点到新节点的哈希表，用空间换时间的方法提高查找速度。

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null)return null;
        //构建辅助头指针
        Node res=new Node(0);
        Node pre=res,p,q=head;
        Map<Node,Node> map=new HashMap<>();
        //复制链表，并构建哈希表
        for(;q!=null;q=q.next){
            p=new Node(q.val);
            pre.next=p;
            pre=p;
            map.put(q,p);
        }
        //构建random指针
        p=res.next;
        q=head;
        for(;p!=null;p=p.next,q=q.next){
            p.random=map.get(q.random);
        }
        return res.next;
    }
}
```

## 解法二：拼接+拆分

构建如下链表：

原结点1->新节点1->原结点2->新节点2->……

这样就不用维护一个哈希表去一一对应原结点和新节点的映射关系了。因为原结点random结点的next结点就是新节点的random结点。

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null)return null;
        Node p,q=head;
        //构建拼接链表
        for(;q!=null;q=p.next){
            p=new Node(q.val);
            p.next=q.next;
            q.next=p;
        }
        //构建random指针
        for(p=head;p!=null;p=p.next.next){
            if(p.random!=null)
                p.next.random=p.random.next;
        }
        //拆分链表
        Node res=head.next;
        Node pre=head;
        for(p=head.next;p.next!=null;p=p.next,pre=pre.next){
            pre.next=p.next;
            p.next=p.next.next;
        }
        pre.next=null;	//封口
        return res; 
    }
}
```





















