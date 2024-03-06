# $$\color{Red}{二叉树中的最大路径和【leetcode124 树形dp】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

二叉树中的 **路径** 被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中 **至多出现一次** 。该路径 **至少包含一个** 节点，且不一定经过根节点。

**路径和** 是路径中各节点值的总和。

给你一个二叉树的根节点 `root` ，返回其 **最大路径和** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

```
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

```
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

 

**提示：**

- 树中节点数目范围是 `[1, 3 * 10^4]`
- `-1000 <= Node.val <= 1000`





### 解析

### 类题【[我的树的最长直径题解](https://www.acwing.com/solution/content/189024/)】

这道题和上面不同的是，这道题是点权，上道题是边权，其次一个是核心模式，一个是 `acm` 模式。

这道题既可以像上一题一样进行多子节点遍历，保存最长边和次长边，更新答案。

但是这道题是二叉树，不需要非得进行多子节点遍历，直接取两个子节点即可。

这两道题都有一个关键点是，负权边会舍去【指的是第一次出现负权边的时候，因为对整体长度是负影响】

即使用 0 和 当前边权进行最大值校验。

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int res = Integer.MIN_VALUE;

    public int dfs(TreeNode u) {
        if (u == null) return 0;
        int left = Math.max(0, dfs(u.left));
        int right = Math.max(0, dfs(u.right));
        res = Math.max(res, u.val + left + right);
        return Math.max(left, right) + u.val;
    }

    public int maxPathSum(TreeNode root) {
        dfs(root);
        return res;
    }
}
```

