## 题目描述	2021年4月26日19:32:01

https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/

> 找出数组中重复的数字。
>
>
> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>
> **示例 1：**
>
> 输入：
>
> [2, 3, 1, 0, 2, 5, 3]
>
> 输出：2 或 3 
>
> **限制：**
>
> 2 <= n <= 100000

## 题解

### 解法一：Set集合

遍历数组，将其存到Set集合中，每次存之前判断是否已经在集合中存在，如果存在那么就返回该值

### 解法二：原地交换

因为在一个长度为 n 的数组 nums 里的所有数字都在 0 ~ n-1 的范围内，这说明有多个数组内的值对应同一个索引。我们可以把每个值放到属于他的索引处，直到发现该值对应的索引处已经有了和他相等的数。

假设数组内的值如下：

![image-20210426194650947](https://gitee.com/mw515031/image/raw/master/image/image-20210426194650947.png)

从索引0处开始进行交换，把0处的值2放到索引2上，把索引2处的1交换到自己这里。

![image-20210426194958844](https://gitee.com/mw515031/image/raw/master/image/image-20210426194958844.png)

交换后如下。但是这时索引0还没得到自己想要的数值0，因此还要继续交换。这里要和索引1去交换。

![image-20210426194936640](https://gitee.com/mw515031/image/raw/master/image/image-20210426194936640.png)

然后和索引3交换。

![image-20210426195229203](https://gitee.com/mw515031/image/raw/master/image/image-20210426195229203.png)

这时索引0便得到了数值0。并且后边的索引1.2.3都已经得到了相应的数值，算法这时跳过到了索引4。

![image-20210426195407186](https://gitee.com/mw515031/image/raw/master/image/image-20210426195407186.png)

这时的索引4对应的数值2就找向了索引2，但是索引2那里已经有了数值2，这说明数值2已经出现了一次了，因此这个值是重复的，需要将其返回。

作者：jyd
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int index,tmp;
        for(int i=0;i<nums.length;i++){
            while(nums[i]!=i){
                index=nums[i];
                if(index==nums[index])
                    return index;
                tmp=nums[i];
                nums[i]=nums[index];
                nums[index]=tmp;
            }
        }
        return -1;
    }
}
```

