

## 题目描述	2020年8月2日18:45:12

https://leetcode-cn.com/problems/count-and-say/description/

给定一个正整数 *n*（1 ≤ *n* ≤ 30），输出外观数列的第 *n* 项。

注意：整数序列中的每一项将表示为一个字符串。

「外观数列」是一个整数序列，从数字 1 开始，序列中的每一项都是对前一项的描述。前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

第一项是数字 1

描述前一项，这个数是 `1` 即 “一个 1 ”，记作 `11`

描述前一项，这个数是 `11` 即 “两个 1 ” ，记作 `21`

描述前一项，这个数是 `21` 即 “一个 2 一个 1 ” ，记作 `1211`

描述前一项，这个数是 `1211` 即 “一个 1 一个 2 两个 1 ” ，记作 `111221`

 

**示例 1:**

```
输入: 1
输出: "1"
解释：这是一个基本样例。
```

**示例 2:**

```
输入: 4
输出: "1211"
解释：当 n = 3 时，序列是 "21"，其中我们有 "2" 和 "1" 两组，"2" 可以读作 "12"，也就是出现频次 = 1 而 值 = 2；类似 "1" 可以读作 "11"。所以答案是 "12" 和 "11" 组合在一起，也就是 "1211"。
```

## 题解

### 解法一：暴力法

**时间复杂度O（n^2）**

```java
/*
 * @lc app=leetcode.cn id=38 lang=java
 *
 * [38] 外观数列
 */

// @lc code=start
class Solution {
    public String countAndSay(int n) {
        //每次聚焦于前两项
        String before = "1";
        StringBuilder after = new StringBuilder();
        // 从第二项开始处理
        for (int i = 2; i <= n; i++) {
            // 从该项第一个数字开始处理
            for (int j = 0; j < before.length(); j++) {
                // 获得第一个数的值并初始化计数次数
                int number = before.charAt(j) - '0';
                int count = 1;
                // 到下一个不相同的数之前到底有多少number
                for (int k = j + 1; k < before.length() && before.charAt(k) - '0' == number; k++) {
                    ++count;
                    ++j;//相同就没必要让j从这里开始
                }
                after.append(count);
                after.append(number);
            }
            before = after.toString();
            after.delete(0, after.length());
        }
        return before;
    }

    public static void main(String[] args) {
        System.out.println(new Solution().countAndSay(6));
    }
}
// @lc code=end

```

