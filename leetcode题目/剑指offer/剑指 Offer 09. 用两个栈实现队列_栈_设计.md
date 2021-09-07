## 题目描述	2021年4月25日22:17:21

https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/

> 用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )
>
>  
>
> 示例 1：
>
> 输入：
> ["CQueue","appendTail","deleteHead","deleteHead"]
> [[],[3],[],[]]
> 输出：[null,null,3,-1]
> 示例 2：
>
> 输入：
> ["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
> [[],[],[5],[2],[],[]]
> 输出：[null,-1,null,null,5,2]
> 提示：
>
> 1 <= values <= 10000
> 最多会对 appendTail、deleteHead 进行 10000 次调用

## 题解

### 解法一：

这个题目中没有考虑队列满时的push操作，所以这个问题中的push函数显得比较简单，否则还要进行队列满判断。

delete函数：

- 如果第二个栈不为空，直接对栈顶进行删除即可模拟出队操作。

- 如果第二个栈为空，那么就要判断第一个栈是否为空
  - 如果为空，说明队列内没有数据，删除失败，返回-1
  - 如果不为空，把第一个栈内的数据全部压到第二个栈中，然后再删除第二个栈的栈顶

```java
class CQueue {

    Stack<Integer> first;
    Stack<Integer> second;
    public CQueue() {
        first=new Stack<>();
        second=new Stack<>();
    }
    
    public void appendTail(int value) {
        first.push(value);
    }
    
    public int deleteHead() {
        if(second.isEmpty()){
            if(first.isEmpty())
                return -1;
            else{
                while(!first.isEmpty()){
                    second.push(first.pop());
                }
            }
        }
        return second.pop();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```

