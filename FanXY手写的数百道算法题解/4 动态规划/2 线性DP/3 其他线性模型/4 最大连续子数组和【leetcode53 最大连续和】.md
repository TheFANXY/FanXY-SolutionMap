# <font color="bb000">最大连续子数组和【leetcode53 最大连续和】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。



**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

 

**提示：**

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`

 

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。



### 解析

这道题就是简单的 `DP` 分析，假设 `f[i]` 表示 以 `nums[i]` 结尾的最大连续子数组和，线性 `DP` 一般都是以结尾划分，因为我们从头开始遍历，只能获取前面的点的信息，那么，很显然根据思考

```java
f[i] =  如下可能的最大值
       -----------------------------------------
①      |  nums[i]		均有公共常数
       |----------------------------------------
②      |  nums[i]  + nums[i - 1]				
③      |  nums[i]  + nums[i - 1] + nums[i - 2]	 
n      |  nums[i]  + ........... + nums[0]
```

显然，他们有公共的常数 `nums[i]` ，而剩余部分，其实从集合角度划分，可以看作 `f[i - 1]` 。

即 `f[i] = nums[i] + f[i-1]` , 而每一项都取决于前一项，即不需要开数组，直接使用变量存储即可。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int res = -0x3f3f3f3f;
        for (int i = 0, last = 0; i < nums.length; i ++) {
            last = Math.max(last, 0) + nums[i];
            res = Math.max(res, last);
        }
        return res;
    }
}
```

这是一个做了一些数组前缀和的题，自然而然的一种想法，但是空间开的大了。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        PriorityQueue<Integer> q = new PriorityQueue<>();
        int [] s = new int[n + 1];
        q.add(0);
        int res = Integer.MIN_VALUE;
        for (int i = 1; i <= n; i ++) {
            s[i] = s[i - 1] + nums[i - 1];
            res = Math.max(res, s[i] - q.peek());
            q.add(s[i]);
        }
        return res;
    }
}
```

