# <font color="bb000">柱状图中最大的矩形【leetcode84 单调栈】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4
```

 

**提示：**

- `1 <= heights.length <=10^5`
- `0 <= heights[i] <= 10^4`



### 解析

先进行常规方法思考，我们的目的是枚举所有的矩形，而为了能找寻一个比较方便枚举的思路，比如已知长找寻高，已知高找寻长，这种策略是必须存在的，要不然只能暴力搜索了。

直觉上可能是双指针，遍历每个柱子的情况下，显然是锁定高去找寻当前柱子的高为矩形的高，然后`向左向右两个指针`同时找寻第一个比它小的柱子，这个柱子`左边/右边` 这两个比它小的柱子排除左右端点之间的区间为长。

但是每移动一次，就进行一次双指针，相当于时间复杂度为 `O(n ^ 2)` ，那么最坏的情况下会超时 =>`10^8`。

那如果能打表，提前记录得话，复杂度就能 `O(n)` 。



### 但是怎么打表？

找寻一个元素在一个有序单列集合左边或右边第一个比它严格小的数， **显然就是单调栈！**

一个指针不断遍历整个数组的过程中，栈维护当前点前的最小点，如果发现遍历过程中，栈顶元素 **大于等于** 遍历到的点的值，就弹出栈顶数字！

为了维护上述的区间，如果左右没有更小的，就用越界的 `-1` 或者 `n` 作为这个点 `左/右` 更小点的位置，这样直接能用来算区间长度来计算矩形的长。

```java
class Solution {
    public int largestRectangleArea(int[] h) {
        LinkedList <Integer> stk = new LinkedList<>();
        int n = h.length;
        int [] left = new int [n];
        int [] right = new int [n];
        for (int i = 0; i < n; i ++) {
            while (!stk.isEmpty() && h[stk.peek()] >= h[i]) stk.pop();
            if (stk.isEmpty()) left[i] = -1;
            else left[i] = stk.peek(); 
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; i --) {
            while (!stk.isEmpty() && h[stk.peek()] >= h[i]) stk.pop();
            if (stk.isEmpty()) right[i] = n;
            else right[i] = stk.peek();
            stk.push(i);
        }
        int res = 0;
        for (int i = 0; i < n; i ++) 
            res = Math.max(res, (right[i] - left[i] - 1) * h[i]);

        return res;
    }
}
```



