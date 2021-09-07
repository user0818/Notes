# 题目描述	2021年4月23日20:29:15

https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

> 字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。
>
> 比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

## 题解

### 解法一：

本题意为将字符串向左移动n个单位，向左多出来的字符补充到字符串右边。

举例如下图：

![image-20210423203507520](https://gitee.com/mw515031/image/raw/master/image/image-20210423203507520.png)

既然要向左移动两格，那么就把这个字符串分成两格部分

![image-20210423203741838](https://gitee.com/mw515031/image/raw/master/image/image-20210423203741838.png)

我们把每个部分都翻转

![image-20210423203820459](https://gitee.com/mw515031/image/raw/master/image/image-20210423203820459.png)

得到的结果和目标结果正好是相反的，遂将整个字符串翻转

![image-20210423203932845](https://gitee.com/mw515031/image/raw/master/image/image-20210423203932845.png)

代码如下：

```java
class Solution {
    public String reverseLeftWords(String s, int n) {

        //变态一行版
        //return new StringBuilder(new StringBuilder(s.substring(0,n)).reverse().toString()+new StringBuilder(s.substring(n,s.length())).reverse().toString()).reverse().toString();
        
        
        StringBuilder sb=new StringBuilder(s);
        for(int i=0;i<n;++i){
            sb.append(s.charAt(i));
        }
        return sb.delete(0,n).toString();
    }
}
```









