# <font color='red'>搜索旋转排序数组2【leetcode81 非单调应用二分】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

# 题目介绍

已知存在一个按非降序排列的整数数组 `nums` ，数组中的值不必互不相同。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转** ，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,4,4,5,6,6,7]` 在下标 `5` 处经旋转后可能变为 `[4,5,6,6,7,0,1,2,4,4]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 `nums` 中存在这个目标值 `target` ，则返回 `true` ，否则返回 `false` 。

你必须尽可能减少整个操作步骤。

 

**示例 1：**

```
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

**示例 2：**

```
输入：nums = [2,5,6,0,0,1,2], target = 3
输出：false
```

 

**提示：**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- 题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
- `-104 <= target <= 104`

 

**进阶：**

- 这是 [搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/description/) 的延伸题目，本题中的 `nums` 可能包含重复元素。
- 这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？



### 解析

这道题是 [我的题解=》搜索旋转排序数组【leetcode33 非单调应用二分】](https://www.acwing.com/solution/content/201406/)的升级版

而唯一的不同就是数组出现了相同元素，那么会发生什么区别？？

事实上，我们无法立马区分的情况下，可以先带入之前的方法，看看会不会出现变化。

此前的做法是

我们可以先观察一下，数组旋转之后的样子 : 【以 [0, 1, 2, 5, 6] 为例子】

```sh
                        6
                    5
                2
            1
        0
index   0   1   2   3   4
```

**旋转之后**

```sh
            6
        5
                        2
                    1
                0     
index   0   1   2   3   4
```

我们发现分段之后两段都各自遵循单调性，如果我们能找到这两段的分界线，满足前一段满足一个性质，后一段也能满足，其实无须单调性也能二分。

其实很轻松就能看出来，分段之后后一段的最大值势必小于第一段的最小值即 `nums[0]` 【原数组无重复元素且单调】

那么就可以以 是否大于等于 `nums[0]` 作为分界条件，最后将两段的边界找出。



### 不同之处

**但是现在有了相同元素会出现一个问题！有可能最后一段的最后一个或多个元素可能和第一段元素的第一个元素，即数组第一个元素相等！**

**即： 靠是否大于等于 `nums[0]` 无法区分两端性！二分失效！**



### 破局之法

其实很简单，根据分析，相等的就是最后这一段而已，我们只要使用一个 `while` 循环把最后和最初元素相等的元素删除即可，如果删完发现数组为空，说明全部相等，用之前的 `nums[0]` 直接和 `target` 进行相等判断即可。不相等直接根据大小在第一段或者第二段进行二分查找，本题结束。



```java
class Solution {
    public boolean search(int[] nums, int target) {
        int maxR = nums.length - 1;
        while (maxR >= 0 && nums[0] == nums[maxR]) maxR --;
        if (maxR < 0) return target == nums[0];

        int l = 0, r = maxR;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (nums[mid] >= nums[0]) l = mid;
            else r = mid - 1;
        }
        if (target >= nums[l]) l = 0;
        else {
            l += 1;
            r = maxR;
        }

        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] < target) l = mid + 1;
            else r = mid;
        }
        return nums[r] == target;
    }
}
```







