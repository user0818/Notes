# 题目描述	2021年4月23日22:24:57

https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/

> 输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。
>
> 例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。
>
> **示例：**
>
> ```
> 给定一个链表: 1->2->3->4->5, 和 k = 2.
> 
> 返回链表 4->5.
> ```

## 题解

### 解法一：双指针

1. 快指针先向后移动k个单位
2. 快慢指针一起向后移动
3. 当快指针指向null结点时，慢指针指向的就是倒数第k个结点。

注意：

1. 第一步注意如果链表长度不为k时的情况。
2. “1”问题

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        if(head==null) return head;
        ListNode p=head,q=head;
        //第一步：快指针移动
        for(int i=0;i<k && p!=null;++i){
            p=p.next;
        }
        //如果节点数不够k，返回原链表
        if(p==null) return head;
        //第二步：快慢指针一起移动注意，由于上边移动了k个单位，这里要用p!=null而不是p.next!=null
        while(p!=null){
            p=p.next;
            q=q.next;
        }
        return q;
    }
}
```

