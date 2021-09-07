# 题目描述	2021年5月15日11:46:26

https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/

> 在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
>
>  
>
> **示例 1:**
>
> ```
> 输入: 
> [
>   [1,3,1],
>   [1,5,1],
>   [4,2,1]
> ]
> 输出: 12
> 解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
> ```
>
>
> 提示：
>
> 0 < grid.length <= 200
>
> 0 < grid[0].length <= 200

# 题解

## 解法一：动态规划

根据题目说明，易得某单元格只可能从上边单元格或左边单元格到达。

设$f(i,j)$为棋盘左上角走至单元格$f(i,j)$的礼物最大累计价值，易得到以下递推关系：$f(i,j)$等于$f(i,j-1)$和$f(i-1,j)$中较大值加上当前单元格礼物价值$grid(i,j)$。
$$
f(i,j)=max[f(i,j-1),f(i-1,j)]+grid(i,j)
$$
因此，可用动态规划解决此问题，以上公式便为转移方程。

![image-20210515150608676](https://gitee.com/mw515031/image/raw/master/image/image-20210515150608676.png)

![image-20210515151040471](https://gitee.com/mw515031/image/raw/master/image/image-20210515151040471.png)

代码如下：

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(i == 0 && j == 0) continue;
                if(i == 0) grid[i][j] += grid[i][j - 1] ;
                else if(j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j]);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```

以上代码逻辑清晰，和转移方程直接对应，但仍可提升效率：当 grid 矩阵很大时， i = 0 或 j = 0 的情况仅占极少数，相当循环每轮都冗余了一次判断。因此，可先初始化矩阵第一行和第一列，再开始遍历递推。

```java
class Solution {
    public int maxValue(int[][] grid) {
        int row=grid.length;
        int colum=grid[0].length;
        //初始化行
        for(int i=1;i<colum;++i)
            grid[0][i]=grid[0][i-1]+grid[0][i];
        //初始化列
        for(int i=1;i<row;++i)
            grid[i][0]=grid[i-1][0]+grid[i][0];
        //正式计算
        for(int i=1;i<row;++i){
            for(int j=1;j<colum;++j)
                grid[i][j]+=Math.max(grid[i-1][j],grid[i][j-1]);
        }
        return grid[row-1][colum-1];
    }
}
```

还有一种解决边界值的方法是直接多申请一行一列的空间（可以理解为最左边和最上边）这样就不会产生边界值问题。因为多申请一行一列，数组下标不会越界，并且利用缺省值为0可以直接比较左边和上边的收益大小，不过这样就不能直接在grid数组上修改，必须重新申请dp数组，算是有得有失吧

```java
class Solution {
    public int maxValue(int[][] grid) {
        int row = grid.length;
        int column = grid[0].length;
        //dp[i][j]表示从grid[0][0]到grid[i - 1][j - 1]时的最大价值
        int[][] dp = new int[row + 1][column + 1];
        for (int i = 1; i <= row; i++) {
            for (int j = 1; j <= column; j++) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]) + grid[i - 1][j - 1];
            }
        }
        return dp[row][column];
    }
}
```

