# $\large\color{Red}{有重复元素的指数型枚举【leetcode90 子集2】}$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud/archives/SolutionMap)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

 

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

 

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`



### 解析

#### 类题 [递归实现指数型枚举 leetcode78 子集1](https://www.acwing.com/solution/content/205643/)

这道题和递归实现指数型枚举不同的是，出现了重复的数字，其实下意识的想法是，能不能把组合型枚举的思路，即 `dfs` 的过程中，记录一个 `st` 即起点，防止 `dfs` 过程中把逆序的个体考虑进去，后面意识到，组合型枚举本质是全排列，这么操作，其实还是有问题。当然我感觉进一步优化是可以做出来的。

那么简单的思路，其实就是每个数用几次这个信息如果知道，可以 `dfs` 的过程中，把这个信息带入，这样就还是指数型枚举，且不会出现重复的情况。



```java
class Solution {

    static List<Integer> path = new ArrayList<>();
    static List<List<Integer>> ans = new ArrayList<>();
    static Map <Integer, Integer> cnt = new HashMap<>();

    public void dfs(int u) {
        if (u > 10) {
            ans.add(new ArrayList(path));
            return;
        }

        for (int i = 0; i <= cnt.getOrDefault(u, 0); i ++) {
            dfs(u + 1);
            path.add(u);
        }

        for (int i = 0; i <= cnt.getOrDefault(u, 0); i ++) {
            path.remove(path.size() - 1);
        }
    }   

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        cnt.clear();
        ans.clear();
        // 哈希表计数
        for (int x : nums) {
            cnt.put(x, cnt.getOrDefault(x, 0) + 1);
        }
        // 搜索
        dfs(-10);
        return ans;
    }
}
```

