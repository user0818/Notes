# 最小生成树--Prim算法

## 说人话

从任意一个节点开始建造一个树。找离这个树最近的结点，并入该树。重复直到所有节点并入该树。

## 举例说明

维护两个数组，vest[]表示结点是否并入生成树；lowcost[]表示生成树到其余各点的代价（树中的任意一个点到其他点的最小值）。

每次把一个新的结点并入生成树时要更新lowcost[]

**执行过程：**

![image-20201218183011921](https://gitee.com/mw515031/image/raw/master/image/image-20201218183011921.png)

假设有图如下：从0开始建造生成树。

![image-20201218181817153](https://gitee.com/mw515031/image/raw/master/image/image-20201218181817153-20210907102836163.png)![image-20201218181857647](https://gitee.com/mw515031/image/raw/master/image/image-20201218181857647.png)

![image-20201218181915154](https://gitee.com/mw515031/image/raw/master/image/image-20201218181915154.png)

![image-20201218181907808](https://gitee.com/mw515031/image/raw/master/image/image-20201218181907808.png)

![image-20201218182011848](https://gitee.com/mw515031/image/raw/master/image/image-20201218182011848.png)

![image-20201218182017434](https://gitee.com/mw515031/image/raw/master/image/image-20201218182017434.png)

![image-20201218182033373](https://gitee.com/mw515031/image/raw/master/image/image-20201218182033373.png)

![image-20201218182042247](https://gitee.com/mw515031/image/raw/master/image/image-20201218182042247.png)

![image-20201218182050900](https://gitee.com/mw515031/image/raw/master/image/image-20201218182050900.png)

![image-20201218182101341](https://gitee.com/mw515031/image/raw/master/image/image-20201218182101341.png)