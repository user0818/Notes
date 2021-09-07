## 题目描述	2021年4月26日22:43:32

https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/

> 给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。
>
> 返回删除后的链表的头节点。
>
> 注意：此题对比原题有改动。书上这里直接给的要删除的结点的指针。
>
> **示例 1:**
>
> 输入: head = [4,5,1,9], val = 5
>
> 输出: [4,1,9]
>
> 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
>
> **示例 2:**
>
> 输入: head = [4,5,1,9], val = 1
>
> 输出: [4,5,9]
>
> 解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
>
> **说明：**
>
> 题目保证链表中节点的值互不相同
>
> 若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点

## 题解

### 解法一：常规删除

定义头结点（空结点），简化删除第一个结点。

定义两个挨边的双指针从头往后遍历，当后指针指向目标数值时，对他进行删除

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
    public ListNode deleteNode(ListNode head, int val) {
        if(head==null) return head;
        ListNode L=new ListNode();
        L.next=head;
        ListNode p=L,q=head;
        while(q!=null){
            if(q.val==val){
                p.next=q.next;
                q=p.next;
            }else{
                p=p.next;
                q=q.next;
            }
        }
        return L.next;
    }
}
```

### 解法二：覆盖法

正常向后遍历，当遇到目标结点时，把它后边的结点值复制到本节点，然后将其后边的那个节点删除。

书上的问题和本题不同：

如果该节点不是尾结点，用覆盖法删除；如果是尾结点，从头遍历。

书上给的是要删除的结点的指针，所以要找到这个结点可以以O(1)的复杂度，配合只有要删除的结点是尾结点时才需要遍历的情况，所以能达到时间复杂度O(1）。但这个题有所不同，这个给的是数值而不是指针，因此即使要删除的不是尾结点，也得遍历。所以这个题不能达到时间复杂度O(1)