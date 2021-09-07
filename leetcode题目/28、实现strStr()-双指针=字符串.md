## 题目描述	2020年7月30日23:48:49

https://leetcode-cn.com/problems/implement-strstr/description/

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回 **-1**。

**示例 1:**

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

**示例 2:**

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

## 题解

### 解法一：暴力法

- 将长度为 L 的滑动窗口沿着 haystack 字符串逐步移动

**时间复杂度：O((N-L)L)**

```java
class Solution {
  public int strStr(String haystack, String needle) {
    int L = needle.length(), n = haystack.length();

    for (int start = 0; start < n - L + 1; ++start) {
      if (haystack.substring(start, start + L).equals(needle)) {
        return start;
      }
    }
    return -1;
  }
}
```

### 解法二：双指针

- haystack的每一个字母和needle的第一个字母比较
- 相同则继续比较needle的后几位
  - 比到needle最后一位仍然相同，则返回与needle第一个字母比较的haystack的下标
- 不同则移向haystack的下一位

**时间复杂度：O((N-L)L)**

```java
/*
 * @lc app=leetcode.cn id=28 lang=java
 *
 * [28] 实现 strStr()
 */

// @lc code=start
class Solution {
    public int strStr(final String haystack, final String needle) {
        if (needle.equals(""))
            return 0;
        final char[] chars = haystack.toCharArray();
        for (int i = 0; i <= chars.length - needle.length(); i++) {
            int k = 0;
            for (int j = i;k<needle.length() && chars[j] == needle.charAt(k); j++, k++) {
            }
            if (k == needle.length())
            return i;
        }
        return -1;
    }

    public static void main(String[] args) {
        Solution solution=new Solution();
        System.out.println(solution.strStr("hello", "ll"));
    }
}
// @lc code=end


```



