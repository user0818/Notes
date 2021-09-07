## 题目描述	2021年4月25日21:27:13

https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/

> 定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。
>
>  
>
> 示例:
>
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
>
>
> 限制：
>
> 0 <= 节点个数 <= 5000
>

## 题解

### 解法一：头插法

把头结点后的每个结点按序插到头结点前边，随即这个刚插入的结点成了新的头结点，以此类推。

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
    public ListNode reverseList(ListNode head) {
        if(head==null)return head;
        ListNode res=head,sup;
        head=head.next;
        res.next=null;
        while(head!=null){
            sup=head.next;
            head.next=res;
            res=head;
            head=sup;
        }
        return res;
    }
}
```

