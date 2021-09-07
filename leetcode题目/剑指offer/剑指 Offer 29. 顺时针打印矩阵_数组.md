## 题目描述	2021年4月27日02:01:22

https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/

> 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。
>
> **示例 1：**
>
> 输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
>
> 输出：[1,2,3,6,9,8,7,4,5]
>
> **示例 2：**
>
> 输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
>
> 输出：[1,2,3,4,8,12,11,10,9,5,6,7]
>
> **限制：**
>
> 0 <= matrix.length <= 100
>
> 0 <= matrix[i].length <= 100

## 题解

### 解法一：

本题的难度不是思路，该干什么题目里都给了，这道题主要考察的是代码能力。

```java
class Solution {
        public int[] spiralOrder(int[][] matrix) {
        //特殊值处理
        if (matrix.length == 0)
            return new int[0];
        //初始化矩阵的高度宽度
        int wide = matrix[0].length;
        int high = matrix.length;
        int[] res = new int[wide * high];
        //初始化每一圈关键值的索引
        int index = 0, right = wide - 1, bottom = high - 1;
        int left = 0, top = 0;
        //每圈循环
        while (left <= right && top <= bottom) {
            //向右
            for (int column = left; column <= right; ++column) {
                res[index++] = matrix[top][column];
            }
            //向下
            for (int row = top + 1; row <= bottom; ++row) {
                res[index++] = matrix[row][right];
            }
            //当矩阵不为方阵时，要判断是否还需要接下来的两步
            if (left < right && top < bottom) {
                //向左
                for (int column = right - 1; column >= left; --column) {
                    res[index++] = matrix[bottom][column];
                }
                //向上
                for (int row = bottom - 1; row > top; --row) {
                    res[index++] = matrix[row][left];
                }
            }
            --right;
            --bottom;
            ++left;
            ++top;
        }
        return res;
    }
}
```

