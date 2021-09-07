## 题目描述	2021年4月27日02:20:58

https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/

> 一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。
>
> 答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。
>
> **示例 1：**
>
> 输入：n = 2
>
> 输出：2
>
> 示例 2：
>
> 输入：n = 7
>
> 输出：21
>
> **示例 3：**
>
> 输入：n = 0
>
> 输出：1
>
> **提示：**
>
> 0 <= n <= 100
>

## 题解

### 解法一：迭代法

递归效率太低，这里使用迭代法。

这里相当于递归的优化。

```java
class Solution {
    public int numWays(int n) {
        if(n<2) return 1;
        int first=1;
        int second=1;
        int third;
        for(int i=2;i<=n;++i){
            third=(first+second)%1000000007;
            first=second;
            second=third;
        }
        return second;
    }
}
```

