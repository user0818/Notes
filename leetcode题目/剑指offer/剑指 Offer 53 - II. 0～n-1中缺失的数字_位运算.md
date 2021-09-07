## 题目描述	2021年4月27日01:43:24

https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/

> 一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。
>
>  
>
> **示例 1:**
>
> 输入: [0,1,3]
>
> 输出: 2
>
> **示例 2:**
>
> 输入: [0,1,2,3,4,5,6,7,9]
>
> 输出: 8
>
> **限制：**
>
> 1 <= 数组长度 <= 10000

## 题解

这个题长度非得用n-1吗？还不如用n更直观点。

长度为n的数组，每个数字在0~n+1之内。

### 解法一：同或

虽然用了位运算，执行速度会更快，但是时间复杂度还是O(N)

就像评论里说的，异或只是为了炫技，只要有序就要用二分。

```java
class Solution {
    public int missingNumber(int[] nums) {
        int res=0;
        for(int i=0;i<nums.length;++i){
            res=res^(i+1);
            res^=nums[i];
        }
        return res;
    }
}
```

### 解法二：二分法

- 排序数组中的搜索问题，首先想到 **二分法** 解决。
- 根据题意，数组可以按照以下规则划分为两部分。
  - **左子数组：** nums[i]  = i；
  - **右子数组：** nums[i] != i；
- 缺失的数字等于 **“右子数组的首位元素”** 对应的索引；因此考虑使用二分法查找 “右子数组的首位元素”

![image-20210427015830610](https://gitee.com/mw515031/image/raw/master/image/image-20210427015830610.png)



```java
class Solution {
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length - 1;
        while(i <= j) {
            int m = (i + j) / 2;
            if(nums[m] == m) i = m + 1;
            else j = m - 1;
        }
        //跳出时，变量 i 和 j 分别指向 “右子数组的首位元素” 和 “左子数组的末位元素” 。因此返回 i 即可。
        return i;
    }
}
```

