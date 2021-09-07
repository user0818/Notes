# 题目描述	2021年5月14日16:03:34

https://leetcode-cn.com/problems/qiu-12n-lcof/

> 求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
>
>  **示例 1：**
>
> 输入: n = 3
>
> 输出: 6
>
> 示例 2：
>
> 输入: n = 9
>
> 输出: 45
>
> **限制：**
>
>1 <= n <= 10000

# 题解

## 解法一

迭代难免要用到各种关键字，但是递归可以避免使用这些关键词。

这里用了一个巧妙的办法，通过==短路==实现递归边界的判断。

```java
class Solution {
    public int sumNums(int n) {
        boolean flag= n>0 && (n+=sumNums(n-1))>0;
        return n;
    }
}
```

