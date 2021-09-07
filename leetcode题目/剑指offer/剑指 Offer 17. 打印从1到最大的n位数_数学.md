# 题目描述	2021年4月23日22:44:44

https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/

> 输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。
>
> 示例 1:
>
> 输入: n = 1
>
> 输出: [1,2,3,4,5,6,7,8,9]
>
>
> 说明：
>
> 用返回一个整数列表来代替打印
>
> n 为正整数

## 题解

### 解法一：

分析：

先算出到底有多少位，再把每一位填上

```java
class Solution {
    public int[] printNumbers(int n) {
        //计算数组长度
        int target=(int)Math.pow(10,n)-1;
        int[] arr=new int[target];
        //填数
        for(int i=1;i<=target;++i){
            arr[i-1]=i;
        }
        return arr;
    }
}
```

