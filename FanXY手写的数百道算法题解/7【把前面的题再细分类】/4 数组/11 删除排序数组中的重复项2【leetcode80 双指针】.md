# <font color='bb000'>删除排序数组中的重复项2【leetcode80 双指针】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 

**说明：**

为什么返回数值是整数，但输出的答案是数组呢？

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `1 <= nums.length <= 3 * 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` 已按升序排列



### 解析

很显然，原地变化，数组删除，不是二分就是双指针，这道题，显然已经排好序，而且如果重复需要保留相同的最多两个，那么直觉上就是双指针类型。

#### 我的三指针想法

一个指针 `k` 用来存数，即 `nums[k]` 是待填入的下一个数字。【严格意义上这个指针不参与后面的移动，只是每次找出需要存的数，存一下然后自增】

而剩下两个指针 `i` 和 `j` ，起点每次重置为相同的位置，然后一旦 `nums[j] == nums[i]` 的情况下，就不断移动 `j` 自增。

当 `j` 停止的情况下，常态下 `[i, j - 1]` 均为相同元素。

那么我们的 `nums[k]` 每次一定存下 `nums[i]` 的值，然后自增。

一旦区间内的数字 `j - i >= 2` ，即存在 `2` 个以上相同的数字，那么 `nums[k]` 再次存下 `nums[i]` 并自增，完成重复元素最多存两次的操作。

然后重置 `i = j` 因为此时 `[i, j - 1]` 均为相同元素， `j` 是新的元素或者是 `nums.length` 即越界情况，重置 `i` 等待下一次相同数字查询范围，或跳出循环。

这个操作明显非常简单，看着字多是因为我想尽可能让大家看一次就完全理解，但是其实思路真的非常简单。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int k = 0;
        int i = 0;
        int j = 0;
        while (i < nums.length) {
            while (j < nums.length && nums[j] == nums[i]) j ++;    
            nums[k ++] = nums[i];
            if (j - i >= 2) nums[k ++] = nums[i];
            i = j;
        }
        return k;
    }
}
```



#### y总的双指针

其实这道题可以完全沿用 [`leetcode26` 删除排序数组中的重复项【leetcode26 双指针】的思路](https://www.acwing.com/solution/content/201016/)

即一个指针 `i` 遍历数组，同时另一个指针 `k` 存数字，当`nums[k - 1]` 和 `nums[k - 2]` 有一个和 `nums[i]` 不同，就 `nums[k ++] = nums[i]`，存下来。这个思路有点像使用 `栈` 的思想在做这道题。

当然如果 `k < 2` 的情况下，可以无脑直接记录并自增 `k`， 因为此时无法满足 `nums[k - 1] 和 nums[k - 2]` 均存在。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int k = 0;
        for (int i = 0; i < nums.length; i ++) {
            if (k < 2 || nums[k - 1] != nums[i] || nums[k - 2] != nums[i])
                nums[k ++] = nums[i];
        }
        return k;
    }
}
```



