## 题目描述	2021年4月27日00:44:53

https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/

> 定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。
>
>  
>
> 示例:
>
> ```
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.min();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.min();   --> 返回 -2.
> ```
>
>
> 提示：
>
> 各函数的调用总次数不超过 20000 次
>

## 题解

本题关注的点在于如何用O(1)来返回栈中的最小元素。

### 解法一：

除了我们要真正存数据的栈，我们再设计一个栈来跟踪当前栈内所有值的最小值。

这个我们新设计的栈保证与数据栈的size相等。栈顶保存==当前==栈内数据的最小值。

**算法流程：**

push(x)：数据栈正常push；min栈将`Math.min(minStack.peek(),x)`入栈

pop(x)：双栈同时pop

min()：peek栈顶

**问题：**

我们为什么不直接用遍量来跟踪最小值？

随着入栈出栈，最小值会变化，本题不是要求返回一次最小值就完事了。

```java
class MinStack {
    Stack<Integer> regStack;
    Stack<Integer> minStack;
    /** initialize your data structure here. */
    public MinStack() {
        regStack=new Stack<>();
        minStack=new Stack<>();
    }
    
    public void push(int x) {
        regStack.push(x);
        if(minStack.isEmpty())
            minStack.push(x);
        else
			minStack.push(Math.min(minStack.peek(),x))
    }
    
    public void pop() {
        minStack.pop();
        regStack.pop();
    }
    
    public int top() {
        return regStack.peek();
    }
    
    public int min() {
        return minStack.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.min();
 */
```

