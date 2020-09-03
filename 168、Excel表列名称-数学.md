## 题目描述	2020年8月26日16:43:26

https://leetcode-cn.com/problems/excel-sheet-column-title/description/

给定一个正整数，返回它在 Excel 表中相对应的列名称。

例如，

```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```

**示例 1:**

```
输入: 1
输出: "A"
```

**示例 2:**

```
输入: 28
输出: "AB"
```

**示例 3:**

```
输入: 701
输出: "ZY"
```

## 题解

### 解法一：

![image-20200826165412352](D:\笔记\leetcode题目\image\image-20200826165412352.png)

![image-20200826165607622](D:\笔记\leetcode题目\image\image-20200826165607622.png)

以26为一个单位，唯一的难点是如何处理余数为0的情况。

解决办法：n-1再取余

```java
/*
 * @lc app=leetcode.cn id=168 lang=java
 *
 * [168] Excel表列名称
 */

// @lc code=start
class Solution {
    public String convertToTitle(int n) {
        StringBuilder sBuilder = new StringBuilder();
        while (n > 0) {
            n -= 1;
            sBuilder.append((char) ('A' + n % 26));
            n /= 26;
        }
        return sBuilder.reverse().toString();
    }

}
// @lc code=end

```

