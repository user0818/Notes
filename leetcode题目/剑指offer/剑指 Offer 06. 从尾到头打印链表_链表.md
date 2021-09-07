## 题目描述	2021年4月25日21:06:49

https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/

> 输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。
>
>  
>
> **示例 1：**
>
> ```
> 输入：head = [1,3,2]
> 输出：[2,3,1]
> ```

## 题解

### 解法一：两次遍历

- 一次遍历统计链表长度
- 第二次遍历从后向前填数组

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
    public int[] reversePrint(ListNode head) {
        if(head==null)
            return new int[0];
        ListNode p=head;
        int count=0;
        while(p!=null){
            ++count;
            p=p.next;
        }
        p=head;
        int[] a=new int[count];
        for(int i=count-1;p!=null;--i,p=p.next){
            a[i]=p.val;
        }
        return a;
    }
}
```

