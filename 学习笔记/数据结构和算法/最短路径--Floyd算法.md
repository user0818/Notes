# 最短路径--Floyd算法

弗洛伊德算法是一个求图最短路径的简单算法。在图中，每个结点可以代表一个实体，而如果两个实体之间有连线就代表这两个实体是连通的。如下图所示，每个实体可以想象成城市，而中间的连线可当成公路。连线上的数字叫做权，代表该条线路的代价（如从起点到终点走该条通路所花费的时间）。

![image-20201217214426580](https://gitee.com/mw515031/image/raw/master/image/image-20201217214426580.png)

我们可以用一个二维数组[$a_{ij}$]表示上述的图的代价，如下所示：

![image-20201217204514216](https://gitee.com/mw515031/image/raw/master/image/image-20201217204514216.png)

[$a_{ij}$]表示从第i个城市到第j个城市所花费的代价，其中规定每个城市到达不和他直连的城市的距离初始化为∞，表示不可到达该城市。很容易理解每个城市到达他自己的距离为0。由于上述二维数组表示的是直连的代价，而且直连并不一定是代价最低的路径（比如第一张图中4到达3，直连代价为12，而绕路到1再到3的代价为5+6=11，明显小于直连的代价）。因此弗洛伊德算法的想法就是尝试利用其它**中间结点到达目标结点**。如果比当前花费要少，那么就更新该路径的代价。即更新的要求为**`a[i][j] > a[i][k]+a[k][j]`**其中k为中间结点。

**缺点：**

弗洛伊德算法不能解决权值为负数的图。因为权值为负数的图没有最短路径。

## 步骤

维护一个路径数组，这个数组的目的是找出路径，它也是一个二维数组，他是根据代价数组进行初始化，初始化规则如下：
$$
Path[i][j]=\left\{
\begin{matrix}
 -1,A[i][j]=0,∞ \\
 i, 其他(i,j之间有边)
\end{matrix}
\right.
$$


![image-20201218000626757](https://gitee.com/mw515031/image/raw/master/image/image-20201218000626757.png)

代价数组的更新上边已经讨论过了，路径数组的更新规则为：如果这条路径改变了，那么就记录该路径是因为哪一个中间结点而改变的，也就是把中间结点记录在路径数组中。
$$
Path[i][j]=k,A[i][j] > A[i][k]+A[k][j]
$$

1. 当1作为中间结点时，更新代价数组和路径数组。

![image-20201218000812339](https://gitee.com/mw515031/image/raw/master/image/image-20201218000812339.png)

2. 当2作为中间结点时，更新代价数组和路径数组。

   ![image-20201218000835951](https://gitee.com/mw515031/image/raw/master/image/image-20201218000835951.png)

3. 当3作为中间结点时，更新代价数组和路径数组。

   ![image-20201218000856088](https://gitee.com/mw515031/image/raw/master/image/image-20201218000856088.png)

4. 当4作为中间结点时，更新代价数组和路径数组。

   ![image-20201218000918047](https://gitee.com/mw515031/image/raw/master/image/image-20201218000918047.png)

   至此，两个数组都变化完毕。此时，如果我想知道从节点2到结点1的最短路径，需要以下步骤：

- Path\[2][1]=**4**，想知道2到1经过哪个路径是最短的，得到了4，即从4到1是最短的，接下来求怎么到4

- Path\[2][**4**]=<u>3</u>，得到3，说明从2到4的最短路径得经过3

- Path\[2][<u>3</u>]=-1，得到-1，说明从2到3有直连的路径。

  因此，路径为2->3->4->1

## 说人话

轮流把每个结点当成中间结点，比较经中间结点绕路的代价是否大于直通的代价。如果是，则更新代价（二维数组）。

```java
//遍历每一个中间结点
for(k=1;k<=n;k++)
    //遍历二维数组
    for(i=1;i<=n;i++)
    for(j=1;j<=n;j++)
    if(e[i][j]>e[i][k]+e[k][j])
    	e[i][j]=e[i][k]+e[k][j];
```















