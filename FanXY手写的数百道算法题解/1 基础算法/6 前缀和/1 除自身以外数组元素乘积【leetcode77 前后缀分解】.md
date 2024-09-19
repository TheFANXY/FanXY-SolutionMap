$$\color{Red}{除自身以外数组元素乘积【leetcode77 前后缀分解】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu/archives/SolutionMap)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个整数数组 `nums`，返回 数组 `answer` ，其中 `answer[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积 。

题目数据 **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内。

请 **不要使用除法，**且在 `O(n)` 时间复杂度内完成此题。

 

**示例 1:**

```
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

**示例 2:**

```
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 

**提示：**

- `2 <= nums.length <= 105`
- `-30 <= nums[i] <= 30`
- **保证** 数组 `nums`之中任意元素的全部前缀元素和后缀的乘积都在 **32 位** 整数范围内

 

**进阶：**你可以在 `O(1)` 的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组 **不被视为** 额外空间。）



### 方法1 额外空间复杂度 `O(N)`

类似前缀和，开两个数组，一个存当前数组前缀数字的乘积，一个存后缀乘积。最后遍历相乘出答案。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int [] l = new int[n];
        int [] r = new int[n];
        l[0] = 1;
        for (int i = 1; i < n; i ++) l[i] = l[i - 1] * nums[i - 1];
        r[n - 1] = 1;
        for (int i = n - 2; i >= 0; i --) r[i] = r[i + 1] * nums[i + 1];
        for (int i = 0; i < n; i ++) nums[i] = l[i] * r[i];
        return nums;
    }
}
```



### 方法2 额外空间复杂度`O(1)`

为了优化，只能开一个数组，这种情况下，只存前缀，倒着遍历，靠一个数字迭代的过程存即可。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int [] p = new int [n];
        for (int i = 0; i < n; i ++) {
            if (i == 0) p[i] = 1;
            else p[i] = p[i - 1] * nums[i - 1];
        }
        for (int i = n - 1, s = 1; i >= 0; i --) {
            p[i] *= s;
            s *= nums[i];
        }
        return p;
    }
}
```

