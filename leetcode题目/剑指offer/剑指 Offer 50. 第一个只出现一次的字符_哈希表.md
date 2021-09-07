## 题目描述	2021年4月26日21:52:38

https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/

> 在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。
>
> **示例:**
>
> s = "abaccdeff"
>
> 返回 "b"
>
> s = "" 
>
> 返回 " "
>
> **限制：**
>
> 0 <= s 的长度 <= 50000
>
> 

## 题解

### 解法一：哈希表

常规解法

```java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character,Integer> map=new HashMap<>();
        for(int i=0;i<s.length();++i){
            char ch=s.charAt(i);
            if(map.containsKey(ch))
                map.put(ch,map.get(ch)+1);
            else
                map.put(ch,1);
        }
        for(int i=0;i<s.length();++i){
            char ch=s.charAt(i);
            if(map.containsKey(ch) && map.get(ch)==1)
                return ch;
        }
        return ' ';
    }
}
```

