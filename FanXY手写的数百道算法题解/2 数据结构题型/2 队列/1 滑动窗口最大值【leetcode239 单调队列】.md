# $$\color{Red}{滑动窗口最大值【leetcode239 单调队列】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 



给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

 

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`



### 推理

这道题从暴力角度看，肯定是维持一个滑动窗口，可以双指针，可以用队列，都一样。

每次滑动遍历窗口取最大值。

但是这么做最坏的情况，是`O(N * N)` 的。

其实当窗口加入一个很大的数，在没出现比他大的情况下，它前面的数都没意义了。因为不可能比他大，而且出队比他早，可以提前出队。

这其实可以看做一个单调队列，那其实最终队列只能是一个单调递减的情况，每次取最左边的，即可满足最大值要求，不需要遍历。`O(N)`

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        LinkedList<Integer> q = new LinkedList<>();
        if (k > n) return new int[0];
        int [] res = new int [n - k + 1];
        int index = 0;
        for (int i = 0; i < n; i ++) {
            if (!q.isEmpty() && i - q.getFirst() + 1 > k) q.pop();
            while (!q.isEmpty() && nums[i] >= nums[q.getLast()]) q.removeLast();
            q.addLast(i);
            if (i >= k - 1) res[index ++] = nums[q.getFirst()];
        }
        return res;
    }
}
```

