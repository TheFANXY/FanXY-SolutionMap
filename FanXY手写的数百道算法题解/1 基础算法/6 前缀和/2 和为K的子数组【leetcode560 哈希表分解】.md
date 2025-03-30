$$\color{Red}{除自身以外数组元素乘积【leetcode77 前后缀分解】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

 

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

 

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`



### 前缀和

这个题最开始想着滑动窗口，后面发现其实左窗口可能会因为右边的值变化，突然能左移了，哪怕排序再做，还是有问题，举例子说明：

如后面出现一个特别大的数，刚好需要把前面删去的负数，才能。

然后判断前缀和，但是没想到优化的方式，先暴力了。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int [] sum = new int [n + 1];
        int res = 0;
        for (int i = 1; i <= n; i ++) sum[i] = sum[i - 1] + nums[i - 1];
        for (int i = 1; i <= n; i ++) {
            for (int j = i; j <= n; j ++) {
                if (sum[j] - sum[i - 1] == k) res ++;
            }
        }
        return res;
    }
}
```

 但是有一个很关键的点，我们上面的做法，本身是枚举终点和起点，但是本质都是后面的前缀和序列，减去前面已经枚举过的前缀和序列。

其实完全可以把前面的序列等于 `k` ，每次找是否存在满足等于 `sum[i] - k` 的情况，有则加前面这个序列的个数即可。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int n = nums.length;
        int [] sum = new int [n + 1];
        int res = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 1; i <= n; i ++) sum[i] = sum[i - 1] + nums[i - 1];
        map.put(0, 1);
        for (int i = 1; i <= n; i ++) {
            res += map.getOrDefault(sum[i] - k, 0);
            map.put(sum[i], map.getOrDefault(sum[i], 0) + 1);
        }
        return res;
    }
}
```

