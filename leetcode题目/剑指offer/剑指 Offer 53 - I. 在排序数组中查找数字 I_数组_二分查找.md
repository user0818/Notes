## 题目描述	2021年4月27日01:18:24

https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/

> 统计一个数字在排序数组中出现的次数。
>
>  
>
> **示例 1:**
>
> 输入: nums = [5,7,7,8,8,10], target = 8
>
> 输出: 2
>
> **示例 2:**
>
> 输入: nums = [5,7,7,8,8,10], target = 6
>
> 输出: 0
>
> **限制：**
>
> 0 <= 数组长度 <= 50000
>

## 题解

### 解法一：遍历

```java
class Solution {
    public int search(int[] nums, int target) {
        int count=0;
        for(int index=0;index<nums.length;++index){
            if(nums[index]==target)
                ++count;
            else if(target<nums[index])
                return count;                
        }
        return count;
    }
}
```

### 解法二：二分法

由于是排序数组，那么只需要找到这个子序列的左右边界就可获得它的长度。

```java
class Solution {
    public int search(int[] nums, int target) {
        // 搜索右边界 right
        int i = 0, j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] <= target) i = m + 1;//这里有等号，说明即使找到了，左指针也要右移，因此找到的是右端点。
            else j = m - 1;
        }
        int right = i;
        // 若数组中无 target ，则提前返回
        if(j >= 0 && nums[j] != target) return 0;
        // 搜索左边界 right
        i = 0; j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] < target) i = m + 1;//这里有等号
            else j = m - 1;				   //说明即使找到了，右指针也要左移，因此找到的是左端点。
        }
        int left = j;
        return right - left - 1;
    }
}
```

