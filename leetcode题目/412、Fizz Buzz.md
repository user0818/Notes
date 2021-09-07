## 题目描述	2020年10月23日22:19:06

https://leetcode-cn.com/problems/fizz-buzz/description/

写一个程序，输出从 1 到 *n* 数字的字符串表示。

1. 如果 *n* 是3的倍数，输出“Fizz”；

2. 如果 *n* 是5的倍数，输出“Buzz”；
3. 如果 *n* 同时是3和5的倍数，输出 “FizzBuzz”。

**示例：**

```
n = 15,

返回:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```

## 题解

### 解法一：常规思路

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> list=new LinkedList<>();
        for(int i=1;i<=n;++i){
            StringBuilder s=new StringBuilder();
            if(i%3==0)
            	s.append("Fizz");
            if(i%5==0)
            	s.append("Buzz");
            if(s.length()==0)
            	s.append(i);
            list.add(s.toString());
        }
        return list;
    }
}
```

这样有个缺点就是很多if导致代码不够优雅，而且若是对应关系增多，if判断将会大量增加，所以可以用map映射来精简if

### 解法二：映射

```java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> list=new LinkedList<>();
        Map<Integer,String> map=new HashMap<>();
        map.put(3,"Fizz");
        map.put(5,"Buzz");
        for(int i=1;i<=n;++i){
            StringBuilder s=new StringBuilder();
            for(int key:map.keySet()){
                if(i%key==0)
                    s.append(map.get(key));
            }
            if(s.length()==0)
                s.append(i);
            list.add(s.toString());
        }
        return list;
    }
}
```

