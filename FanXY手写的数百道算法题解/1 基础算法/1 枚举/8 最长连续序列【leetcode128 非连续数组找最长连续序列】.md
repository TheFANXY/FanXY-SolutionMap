# <font color="bb000">最长连续序列【leetcode128 非连续数组找最长连续序列】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`



暴力 ： 肯定是枚举 ，双重循环找前面有没有比他小1的，有的话接着找，一直找到头，然后再找比他大的，找到尾。

但是这么想太麻烦，肯定最好是排序从头找，然后找到一批，这批就可以清除了。

但是还会出现重复的数，为了轻松最好去重。

即 =》 `set` 去重，然后遍历找到一组通过 `set` 直接批量删除。

```java
class Solution {

    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int x : nums) set.add(x);
        int res = 0;
        for (int x : nums) {
            if (set.contains(x) && !set.contains(x - 1)) {
                int y = x;
                while (set.contains(y + 1)) {
                    set.remove(y ++);
                }
                res = Math.max(res, y - x + 1);
            }
        }
        return res;
    }
}
```

