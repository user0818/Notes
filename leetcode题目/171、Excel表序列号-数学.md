## 题目描述	2020年9月8日18:12:31

https://leetcode-cn.com/problems/excel-sheet-column-number/description/

给定一个Excel表格中的列名称，返回其相应的列序号。

例如，

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

**示例 1:**

```
输入: "A"
输出: 1
```

**示例 2:**

```
输入: "AB"
输出: 28
```

**示例 3:**

```
输入: "ZY"
输出: 701
```

## 题解

### 解法一：

相当于26进制转换为10进制

**时间复杂度：O(N)**

**空间复杂度：O(1)**

```java
/*
 * @lc app=leetcode.cn id=171 lang=java
 *
 * [171] Excel表列序号
 */

// @lc code=start
class Solution {
    public int titleToNumber(String s) {
        int len = s.length();
        int res = 0;
        for (int i = len - 1, j = 1; i >= 0; i--, j *= 26) {
            int tmp = s.charAt(i) - 'A' + 1;
            res = tmp * j + res;
        }
        return res;
    }
}
// @lc code=end

```

