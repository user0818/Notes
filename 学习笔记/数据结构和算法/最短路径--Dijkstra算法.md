# 最短路径--Dijkstra算法

不同于Floyd算法可以求出所有点到所有点的最短路径，Dijkstra算法往往是用来求一个点到其他各个点的最短路径，也就是“单源最短路径问题”。该算法使用**<u>贪心思想</u>**实现。

和Floyd算法相似，Dijkstra算法也是通过**选取中间结点到各节点的距离**与**旧距离**进行比较来更新路径的。但与之不同的是，Floyd算法是轮流选取中间结点（一个个试，没有规律，顺序可以不同），而Dijkstra算法选取中间结点是有讲究的，即选取**离目前选中的结点集最近的结点**。

## 算法思想

把结点集分为两部分，一部分为已更新完源点到其最短路径的结点集（设为S）；另一部分为还没找到到他们的最短路径的结点集（设为T）。

1. 假设从v0出发，S中就只有v0一个点，此时v0既是源点又是顶点vu。
2. 接下来从T中寻找哪个点通过vu中转到v0最近，将其并入S，并把它标记为顶点vu。
3. 每当S并入一个新的顶点vu，都要修改v0到集合T中顶点的最短路径长度值。（因为以前达不到的点现在或许可以通过vu到达该点，或者通过vu为中转，新的路径比原来的更近）
4. 重复第2，3步。直到所有节点并入S。

## 说人话

把S当成一个生成树，每次找离这个树最近的点并入生成树，把这个点当成中转到其他点会不会更近，如果近了就更新距离。

## 举例说明

维护三个数组，dist[]记录源点到其他点的距离；path[]记录以哪个节点为中间结点到达该节点；set[]记录这个点是否并入S。

假设有图如下：

![image-20201218135232863](https://gitee.com/mw515031/image/raw/master/image/image-20201218135232863.png)



此时S={0}，T={1,2,3,4,5,6}，各数组数据如下：

![image-20201218165355345](https://gitee.com/mw515031/image/raw/master/image/image-20201218165355345.png)

![image-20201218165501468](https://gitee.com/mw515031/image/raw/master/image/image-20201218165501468.png)

![image-20201218175245317](https://gitee.com/mw515031/image/raw/master/image/image-20201218175245317.png)

![image-20201218165409919](https://gitee.com/mw515031/image/raw/master/image/image-20201218165409919.png)

![image-20201218165424456](https://gitee.com/mw515031/image/raw/master/image/image-20201218165424456.png)

![image-20201218175042625](https://gitee.com/mw515031/image/raw/master/image/image-20201218175042625.png)

![image-20201218175324102](https://gitee.com/mw515031/image/raw/master/image/image-20201218175324102.png)![image-20201218175203207](https://gitee.com/mw515031/image/raw/master/image/image-20201218175203207.png)

![image-20201218175409514](https://gitee.com/mw515031/image/raw/master/image/image-20201218175409514.png)

![image-20201218175429545](https://gitee.com/mw515031/image/raw/master/image/image-20201218175429545.png)

![image-20201218175506673](https://gitee.com/mw515031/image/raw/master/image/image-20201218175506673.png)![image-20201218175517103](https://gitee.com/mw515031/image/raw/master/image/image-20201218175517103.png)

![image-20201218175528132](https://gitee.com/mw515031/image/raw/master/image/image-20201218175528132.png)

![image-20201218175536285](https://gitee.com/mw515031/image/raw/master/image/image-20201218175536285.png)

![image-20201218175622158](https://gitee.com/mw515031/image/raw/master/image/image-20201218175622158.png)![image-20201218175629438](https://gitee.com/mw515031/image/raw/master/image/image-20201218175629438.png)

![image-20201218175645474](https://gitee.com/mw515031/image/raw/master/image/image-20201218175645474.png)

![image-20201218175656492](https://gitee.com/mw515031/image/raw/master/image/image-20201218175656492.png)

![image-20201218175739715](https://gitee.com/mw515031/image/raw/master/image/image-20201218175739715.png)![image-20201218175747370](https://gitee.com/mw515031/image/raw/master/image/image-20201218175747370.png)

![image-20201218175800484](https://gitee.com/mw515031/image/raw/master/image/image-20201218175800484.png)

![image-20201218175812692](https://gitee.com/mw515031/image/raw/master/image/image-20201218175812692.png)

![image-20201218175837456](https://gitee.com/mw515031/image/raw/master/image/image-20201218175837456.png)此时所有节点均已并入最短路径，求解结束。

![image-20201218175921701](https://gitee.com/mw515031/image/raw/master/image/image-20201218175921701.png)

![image-20201218180002888](https://gitee.com/mw515031/image/raw/master/image/image-20201218180002888.png)

求解路径的方法：比如要从0到达编号为6的点。

1. path[6]=4，要到达6，要以4为中转节点
2. path[4]=5，要到达4，要以5为中转节点
3. path[5]=2，同上
4. path[2]=1，同上
5. path[1]=0，以0为中转节点，又因为0是出发点，所以求解完毕。

反向输出上边的序列0->1->2->5->4->6























