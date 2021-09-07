## 题目描述	2021年4月27日01:39:51

https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/

> 从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。
>
>  
>
> 示例 1:
>
> 输入: [1,2,3,4,5]
>
> 输出: True
>
>
> 示例 2:
>
> 输入: [0,0,1,2,5]
>
> 输出: True
>
>
> 限制：
>
> 数组长度为 5 
>
> 数组的数取值为 [0, 13] .
>

## 题解

### 解法一

由于大小王可以当做**赖子**来代替任何牌，那么就统计这个顺子中间到底缺多少空缺需要赖子填补即可。

```java
class Solution {
    public boolean isStraight(int[] nums) {
        //排序
        Arrays.sort(nums);

        //大小王的数量
        int ghost=0;
        for(int i=0;i<nums.length-1;++i){
            //统计大小王数量
            if(nums[i]==0)
                ++ghost;
            else {
                //如果要构成顺子，有几个空缺需要小王填充
                int blank=nums[i+1]-nums[i]-1;
                //有相同的牌，肯定构不成顺子
                if(blank<0)
                    return false;
                //小王数量不足以填补空缺
                if(blank>ghost)
                    return false;
                else
                    ghost-=blank;   //拿小王填空
            }
        }
        return true;
    }
}
```

