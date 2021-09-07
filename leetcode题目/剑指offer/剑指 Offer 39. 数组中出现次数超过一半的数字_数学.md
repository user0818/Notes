## 题目描述	2021年4月26日15:14:06

https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/

> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
>
> 你可以假设数组是非空的，并且给定的数组总是存在多数元素。
>
>  
>
> **示例 1:**
>
> 输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
>
> 输出: 2
>
> **限制：**
>
> 1 <= 数组长度 <= 50000
>

## 题解

### 解法一：哈希表法

遍历数组统计次数，找出出现次数最多的

### 解法二：排序法

排序后，中点必定为数组的众数

### 解法三：摩尔投票法

正负抵消。

把第一个出现的数当做候选数，每出现一个与该数相同的数，计数加一；反之计数减一。

当计数为0，且又出现一个非该数时，把这个新出现的数记为候选的数，并计数为1。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int len=nums.length;
        int count=1;
        int occurMaxNumber=nums[0];
        for(int i=1;i<len;++i){
            if(nums[i]==occurMaxNumber)
                ++count;
            else
                --count;
            if(count<0){
                count=1;
                occurMaxNumber=nums[i];
            }
        }
        return occurMaxNumber;
    }
}
```

