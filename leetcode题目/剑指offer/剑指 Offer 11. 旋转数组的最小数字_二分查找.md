## 题目描述	2021年4月27日01:28:40

https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/

> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  
>
> **示例 1：**
>
> 输入：[3,4,5,1,2]
>
> 输出：1
>
> **示例 2：**
>
> 输入：[2,2,2,0,1]
>
> 输出：0

## 题解

### 解法一：遍历

```java
class Solution {
    public int minArray(int[] numbers) {
        for(int i=0;i<numbers.length-1;++i){
            if(numbers[i]>numbers[i+1])
                return numbers[i+1];
        }
        return numbers[0];
    }
}
```

### 解法二：二分查找

一个包含重复元素的升序数组在经过旋转之后，可以得到下面可视化的折线图：

![fig1](https://gitee.com/mw515031/image/raw/master/image/1.png)

其中横轴表示数组元素的下标，纵轴表示数组元素的值。图中标出了最小值的位置，是我们需要查找的目标。

我们考虑数组中的最后一个元素 xx：在最小值右侧的元素，它们的值一定都小于等于 xx；而在最小值左侧的元素，它们的值一定都大于等于 xx。因此，我们可以根据这一条性质，通过二分查找的方法找出最小值。

在二分查找的每一步中，左边界为low，右边界为high，区间的中点为pivot，最小值就在该区间内。我们将中轴元素numbers[pivot] 与右边界元素 numbers[high] 进行比较，可能会有以下的三种情况：

第一种情况是 number[pivot]<number[high]。如下图所示，这说明 number[pivot]是最小值右侧的元素，因此我们可以忽略二分查找区间的右半部分。

![fig2](https://gitee.com/mw515031/image/raw/master/image/2.png)

第二种情况是  number[pivot]>number[high]。如下图所示，这说明 number[pivot] 是最小值左侧的元素，因此我们可以忽略二分查找区间的左半部分。

![fig3](https://gitee.com/mw515031/image/raw/master/image/3.png)

第三种情况是 number[pivot]==number[high]。如下图所示，由于重复元素的存在，我们并不能确定 number[pivot] 究竟在最小值的左侧还是右侧，因此我们不能莽撞地忽略某一部分的元素。我们唯一可以知道的是，由于它们的值相同，所以无论 number[high] 是不是最小值，都有一个它的「替代品」number[pivot]，因此我们可以忽略二分查找区间的右端点。

![fig4](https://gitee.com/mw515031/image/raw/master/image/4.png)

当二分查找结束时，我们就得到了最小值所在的位置。

代码如下：

```java
class Solution {
    public int minArray(int[] numbers) {
        int low = 0;
        int high = numbers.length - 1;
        while (low < high) {
            int pivot = low + (high - low) / 2;
            if (numbers[pivot] < numbers[high]) {
                high = pivot;
            } else if (numbers[pivot] > numbers[high]) {
                low = pivot + 1;
            } else {
                high -= 1;
            }
        }
        return numbers[low];
    }
}
```

转自leetcode官方