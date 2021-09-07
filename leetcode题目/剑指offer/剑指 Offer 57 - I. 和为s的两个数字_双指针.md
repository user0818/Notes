## 题目描述	2021年4月26日19:58:07

https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/

> 输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。
>
>  
>
> **示例 1：**
>
> 输入：nums = [2,7,11,15], target = 9
>
> 输出：[2,7] 或者 [7,2]
>
> **示例 2：**
>
> 输入：nums = [10,26,30,31,47,60], target = 40
>
> 输出：[10,30] 或者 [30,10]
>
> **限制：**
>
> 1 <= nums.length <= 10^5
>
> 1 <= nums[i] <= 10^6

## 题解

### 解法一：双指针

双指针分别指向数组的头部和尾部，计算他俩的和。

若他们之和太大，就把右指针向左移动；若他们俩之和太小，就把左指针向右移动；如果正好等于目标，就返回这两个数。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int i=0,j=nums.length-1;
        int s;
        while(i<j){
            s=nums[i]+nums[j];
            if(s>target)
                j--;
            else if(s<target)
                i++;
            else
                return new int[]{nums[i],nums[j]};
        }
        return null;
    }
}
```

