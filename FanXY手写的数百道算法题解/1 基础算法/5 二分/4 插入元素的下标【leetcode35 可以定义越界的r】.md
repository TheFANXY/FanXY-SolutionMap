# <font color='red'>插入元素的下标【leetcode35 可以定义越界的r】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

# 题目介绍

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 

**提示:**

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-10^4 <= target <= 10^4`



### 解析

这道题二分部分就不说了，只是想说明二分如果求的位置是数组越界的位置，二分边界也可以定义到那里，还能省去边界判断，因为如果越界就相当于左或者右指针一直往另一个方向靠，直到相等，所以越界的情况，也能够到，这道题想说明如果定义左右指针位置定义错了，也是会发生大错误，因为会一直靠到一个边界直到重叠，开大了就会出现越界。



```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = l + r >> 1;
            if (nums[mid] >= target) r = mid;
            else l = mid + 1;
        }
        return l;
    }
}
```

