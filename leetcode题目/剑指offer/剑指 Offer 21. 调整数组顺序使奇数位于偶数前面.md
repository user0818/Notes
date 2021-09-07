## 题目描述	2021年4月26日21:04:23

> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。
>
> **示例：**
>
> 输入：nums = [1,2,3,4]
>
> 输出：[1,3,2,4] 
>
> 注：[3,1,2,4] 也是正确的答案之一。
>
> **提示：**
>
> 0 <= nums.length <= 50000
>
> 1 <= nums[i] <= 10000

## 题解

### 解法一：双指针

参考快速排序，分别设置两个指针从两端到中间进行。由于要使奇数位于偶数后边，则需要左指针遇到偶数时停止，右指针遇到奇数时停止，然后将二者交换。

```java
class Solution {
    public int[] exchange(int[] nums) {
        if(nums.length<2) return nums;
        int odd,even;
        for(int i=0,j=nums.length-1;i<j;++i,--j){
            while(nums[i]%2==1 && i<j) ++i;
            while(nums[j]%2==0 && i<j) --j;
            if(i<j){
                int tmp=nums[i];
                nums[i]=nums[j];
                nums[j]=tmp;
            }
        }
        return nums;
    }
}
```

