## 题目描述	2021年4月27日00:34:15

https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/

> 	写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。
>
>  
>
> **示例:**
>
> 输入: a = 1, b = 1
>
> 输出: 2
>
> **提示：**
>
> a, b 均可能是负数或 0
>
> 结果不会溢出 32 位整数

## 题解

### 解法一：位运算

考察加法在二进制中是怎么体现的。

二进制的加法有四种情况：

![image-20210427003730107](https://gitee.com/mw515031/image/raw/master/image/image-20210427003730107.png)

观察发现，**无进位和** 与 **异或运算** 规律相同，**进位** 和 **与运算** 规律相同（并需左移一位）。因此，无进位和 n*n* 与进位 c*c* 的计算公式如下；

![image-20210427003800222](https://gitee.com/mw515031/image/raw/master/image/image-20210427003800222.png)

我们这里就把两个加数的和转化为==非进位和==与==进位和==相加。重点是我们在这里看到了循环。因此我们可以循环地求n和c，直至进位和为0.

注意：n和c的值要同步更新，这就是下方要引入tmp的原因

```java
class Solution {
    public int add(int a, int b) {
        while(b!=0){
            int tmp=a^b;
            b=(a&b)<<1;
            a=tmp;
        }
        return a;
    }
}
```

