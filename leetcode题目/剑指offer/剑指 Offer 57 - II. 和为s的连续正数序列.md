## 题目描述	2021年4月26日13:26:26

https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/

> 输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。
>
> 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
>
>  
>
> **示例 1：**
>
> 输入：target = 9
>
> 输出：[[2,3,4],[4,5]]
>
> **示例 2：**
>
> 输入：target = 15
>
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]
>
> **限制：**
>
> 1 <= target <= 10^5

## 题解

### 解法一

**关键词：**递增，连续。

假设我们从头开始数数，那么我们得到的第一个数就是1。

这时如果target比1大，那么我们就要把下一个数加入到这个序列中，构成[1,2]。如果序列中数值的和还是比target小，就再次把下一个数添加到列表中。

而如果列表的和比target小，说明列表里的数太多了，得删除一个（注①），于是就把第一个数从列表里删除。

**注①：**由于题目要求是连续序列，那么只能删掉首部或者尾部，而尾部的数字是上一步因为序列和太小而添进来的，如果删了就又回到了上一步，陷入了死循环；况且，于情于理要删除也得先删除最小的数字，即序列首部的数字。

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        //定义二维数组
        ArrayList<ArrayList<Integer>> res=new ArrayList<>();
        //双指针跟踪序列长度
        int start=1,end=2;
        //序列尾部肯定不会大于target
        while(end<target){
            int sum=0;
            //申请临时列表
            ArrayList<Integer> tmp=new ArrayList<Integer>();
            //计算临时列表之和
            for(int i=start;i<=end;++i){
                tmp.add(i);
                sum+=i;
            }
            //判断临时列表和与target关系，并因此决定是否添加到结果集中
            if(sum==target){
                res.add(tmp);
                start+=1;
            }   
            else if(sum<target)
                end+=1;
            else
                start+=1;
        }
        //把二维列表转化为二维数组
        int [][] resArrays=new int[res.size()][];
        for(int i=0;i<res.size();++i){
            resArrays[i]=new int[res.get(i).size()];
            for(int j=0;j<res.get(i).size();++j){
                resArrays[i][j]=res.get(i).get(j);
            }
        }
        return resArrays;
    }
}
```

