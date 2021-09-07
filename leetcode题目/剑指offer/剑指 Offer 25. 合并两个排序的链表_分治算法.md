## 题目描述	2021年4月25日22:12:19

https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/

> 输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
> 示例1：
>
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> 限制：
>
> 0 <= 链表长度 <= 1000

## 题解

### 解法一：尾插法

申请一个头结点（新的链表），将两个链表按大小依次插入到这个新链表的尾部。

如果有一方插完了，不要忘了把没插完的那个链表整个挂到新链表尾部。

ps：头结点在这个问题中十分便利

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
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode res=new ListNode();
        ListNode sup,p=res;
        //尾插入
        while(l1!=null && l2!=null){
            if(l1.val<l2.val){
                sup=l1.next;
                p.next=l1;
                p=p.next;
                l1=sup;
            }else{
                sup=l2.next;
                p.next=l2;
                p=p.next;
                l2=sup;
            }
        }
        //处理剩下的链表
        if(l1!=null)
            p.next=l1;
        if(l2!=null)
            p.next=l2;
        
        return res.next;
    }
}
```

