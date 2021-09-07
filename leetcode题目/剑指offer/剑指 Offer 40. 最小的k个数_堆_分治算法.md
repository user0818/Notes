## 题目描述	2021年4月27日00:57:40

https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/

> 输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。
>
>  
>
> 示例 1：
>
> 输入：arr = [3,2,1], k = 2
>
> 输出：[1,2] 或者 [2,1]
>
> 示例 2：
>
> 输入：arr = [0,1,2,1], k = 1
>
> 输出：[0]
>
>
> 限制：
>
> 0 <= k <= arr.length <= 10000
>
> 0 <= arr[i] <= 10000

## 题解

### 解法一：自建大顶堆

Java中的PriorityQueue可以用来建立大顶堆，但默认是递增的（小顶堆），要重写Comparator的compare方法才能变成大顶堆。

用一个size为k的大顶堆维护最小的k个值，如果接下来要插入的数比堆顶要小，就把堆顶出队，把该数入队。

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length==k)   return arr;
        int []res=new int[k];
        if(k==0) return res;

        PriorityQueue<Integer> queue=new PriorityQueue<>(new Comparator<>(){
            public int compare(Integer num1,Integer num2){
                return num2-num1;
            }
        });
        for(int i=0;i<k;++i){
            queue.offer(arr[i]);
        }
        for(int i=k;i<arr.length;++i){
            if(arr[i]<queue.element()){
                queue.remove();
                queue.offer(arr[i]);
            }
        }
        for(int i=0;i<k;++i){
            res[i]=queue.remove();
        }
        return res;
    }
}
```

