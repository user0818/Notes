## 题目描述	2021年4月26日20:58:11

https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/

> 0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。
>
> 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
>
>  
>
> **示例 1：**
>
> 输入: n = 5, m = 3
>
> 输出: 3
>
> **示例 2：**
>
> 输入: n = 10, m = 17
>
> 输出: 2
>
> **限制：**
>
> 1 <= n <= 10^5
>
> 1 <= m <= 10^6

## 题解

### 解法一

我们有n个数，下标从0到n-1，然后从`index=0`开始数，每次数m个数，最后看能剩下谁。我们假设能剩下的数的**下标**为y，则我们把这件事表示为

```
f(n,m) = y
```

这个y到底表示了啥呢？注意，y是下标，所以就意味着你从`index=0`开始数，数y+1个数，然后就停，停谁身上谁就是结果。

行了，我们假设`f(n-1,m)=x`，然后来找一找`f(n,m)`和`f(n-1,m)`到底啥关系。

`f(n-1,m)=x`意味着啥呢？意味着有n-1个数的时候从`index=0`开始数，数x+1个数你就找到这结果了。那我不从`index=0`开始数呢？比如我从`index=i`开始数？那很简单，你把上面的答案也往后挪i下，就得到答案了。当然了，你要是挪到末尾了你就取个余，从头接着挪。

于是我们来思考`f(n,m)`时考虑以下两件事：

1. 有n个数的时候，要划掉一个数，然后就剩n-1个数了呗，那划掉的这个数，**下标**是多少？
2. 划完了这个数，往后数，数x+1个数，停在谁身上谁就是我们的答案。当然了，数的过程中你得取余

**问题一**：有n个数的时候，划掉了谁？**下标**是多少？

因为要从0数m个数，那最后肯定落到了下标为m-1的数身上了，但这个下标可能超过我们有的最大下标（n-1）了。所以攒满n个就归零接着数，逢n归零，所以要模n。

所以有n个数的时候，我们划掉了下标为`(m-1)%n`的数字。

**问题二**：我们划完了这个数，往后数x+1下，能落到谁身上呢，它的下标是几？

你往后数x+1，它下标肯定变成了`(m-1)%n +x+1`，和第一步的想法一样，你肯定还是得取模，所以答案为`[(m-1)%n+x+1]%n`，则

```erlang
f(n,m)=[(m-1)%n+x+1]%n
```

其中`x=f(n-1,m)`

我们化简它！

定理一：两个正整数a，b的和，模另外一个数c，就等于它俩分别模c，模完之后加起来再模。

```perl
(a+b)%c=((a%c)+(b%c))%c
```

定理二：一个正整数a，模c，模一遍和模两遍是一样的。

```
a%c=(a%c)%c
```

你稍微一琢磨就觉得，嗯，说得对。

所以

```perl
f(n,m)=[(m-1)%n+x+1]%n
      =[(m-1)%n%n+(x+1)%n]%n
      =[(m-1)%n+(x+1)%n]%n
      =(m-1+x+1)%n
      =(m+x)%n
```

剩下的故事你们就都知道了。

==转自leetcode评论区Lucien大佬==

代码如下：

```java
class Solution {
    public int lastRemaining(int n, int m) {
        if (n == 1) {
            return 0;
        }
        int x = lastRemaining(n - 1, m);
        return (m + x) % n;
    }
}
```

