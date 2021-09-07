## 题目描述	2021年4月23日22:52:10

https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/

> 请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
>
>  
>
> 示例 1：
>
> 输入：s = "We are happy."
>
> 输出："We%20are%20happy."

## 题解

### 解法一：遍历法

利用StringBuilder可变的特点，遍历字符串，每当遇到 空格 就append(''%20')

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder res = new StringBuilder();
        for(Character c : s.toCharArray())
        {
            if(c == ' ') res.append("%20");
            else res.append(c);
        }
        return res.toString();
    }
}
```

