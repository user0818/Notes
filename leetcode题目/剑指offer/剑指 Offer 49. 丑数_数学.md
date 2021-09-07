# 题目描述	2021年5月27日15:46:48

https://leetcode-cn.com/problems/chou-shu-lcof/

> 我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。
>
>  
>
> **示例:**
>
> 输入: n = 10
>
> 输出: 12
>
> 解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
>
> **说明:**  
>
> 1 是丑数。
>
> n 不超过1690。

# 题解

> 丑数的递推性质： 丑数只包含因子 2, 3, 5 ，因此有 “丑数 = 某较小丑数 × 某因子” （例如：10=5×2）。
>

按常规的想法，自然会想到判断一个数他是不是丑数，然后遍历。但是仔细观察发现，丑数之间是有规律的。

假设我们手里有一个丑数，那么对它\*2，\*3，\*5依然也是丑数。

## 解法一：小顶堆

按照上述的思想，我们从第一个丑数`1`开始，把它分别乘2,3,5，随后得到新的三个丑数。再对这些丑数继续乘2,3,5，以此类推。

由于按这个步骤添加的值不是有序的，所以要用小顶堆来存储过程。

**算法步骤：**

1. `1`入队
2. 取出堆顶元素，把他们分别乘2,3,5
3. 检查新获得的三个数是否已经存在，不存在则入队
4. 重复2过程，直到取出第n个元素

**复杂度分析**

时间复杂度：O(nlogn)。得到第 n 个丑数需要进行 n 次循环，每次循环都要从最小堆中取出 1 个元素以及向最小堆中加入最多 3 个元素，因此每次循环的时间复杂度是 O(log n+log 3n)=O(logn)，总时间复杂度是 O(nlogn)。

空间复杂度：O(n)。空间复杂度主要取决于最小堆和哈希集合的大小，最小堆和哈希集合的大小都不会超过 3n。

```java
class Solution {
    public int nthUglyNumber(int n) {

        int[] factors=new int[]{2,3,5};
        PriorityQueue<Long> heap = new PriorityQueue<>();
        Set<Long> set=new HashSet<Long>();
        heap.add(1L);
        set.add(1L);
        int ugly=0;
        for(int i=0;i<n;++i){
            Long cur=heap.remove();
            ugly=cur.intValue();
            for(int factor:factors){
                Long newUglyNumber=factor*cur;
                if(!set.contains(newUglyNumber)){
                    set.add(newUglyNumber);
                    heap.add(newUglyNumber);
                }
            }
        }
        return ugly;
    }
}
```

## 解法二：动态规划

![image-20210527164242657](https://gitee.com/mw515031/image/raw/master/image/image-20210527164242657.png)

![image-20210527164303602](https://gitee.com/mw515031/image/raw/master/image/image-20210527164303602.png)

==个人理解：==

这里不同于法一把所有可能的候选丑数入队，而是精确的找到下一个丑数。

这里把关注点放在了三个索引上。我们知道下一个丑数一定可以由当前的丑数推出来，所以让abc指向当前的丑数。abc可以看做步长，分别为235。

我们通过abc索引，可以找到三个候选的丑数。但是因为我们要找的X~n~是比X~n-1~大的最小丑数，所以我们要取三个候选丑数的最小值。

因为abc分别代表一个特定的步长，一旦其指定的索引乘以步长被选中成为下一个丑数，那么这个索引就要前进一格。

**复杂度分析：**

时间复杂度 O(N) ： 其中 N=n ，动态规划需遍历计算 dp 列表。

空间复杂度 O(N) ： 长度为 N 的 dp 列表使用O(N) 的额外空间。

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp=new int[n];
        dp[0]=1;
        int a=0,b=0,c=0;
        for(int i=1;i<n;i++){
            // 找到三个候选丑数
            int nextCandidateUgly2=dp[a]*2;
            int nextCandidateUgly3=dp[b]*3;
            int nextCandidateUgly5=dp[c]*5;
            // 取最小的候选丑数
            int nextUgly=Math.min(Math.min(nextCandidateUgly2,nextCandidateUgly3),nextCandidateUgly5);
            dp[i]=nextUgly;
            // 这次的丑数是乘几得来的
            if(dp[i]==nextCandidateUgly2) a++;
            if(dp[i]==nextCandidateUgly3) b++;
            if(dp[i]==nextCandidateUgly5) c++;
        }
        return dp[n-1];
    }
}
```





















