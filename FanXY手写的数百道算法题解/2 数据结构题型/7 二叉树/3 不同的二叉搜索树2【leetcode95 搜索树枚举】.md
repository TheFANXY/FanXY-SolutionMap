# $$\color{Red}{不同的二叉搜索树2【leetcode95 搜索树】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 8`



### 解析

从递归的角度去考虑，对于一个二叉搜索树来说，左边的节点一定小于根节点，右边的节点一定大于根节点。

故我们可以枚举从 `[1 -> n]` 作为根节点的二叉搜索树，从递归的思想考虑，如果枚举的节点是 `i` ，显然，它的左子树应当是 `[l, i - 1]` 这段，而右子树来自 `[i + 1, r]` ，由此可见，我们只需要递归的处理左子树这段和右子树这段。

为了满足当出现左子树或者右子树为空的情况，仍存储节点，需要进行 当 `l > r` 将空节点 `null` 放入 `ArrayList` ，如果不放，进行遍历的情况下，直接列表为空，不会进行节点的添加了。

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

    static List<TreeNode> dfs(int l, int r) {
        List<TreeNode> res = new ArrayList<>();
        if (l > r) {
            res.add(null);
            return res;
        }

        for (int i = l; i <= r; i ++) {
            List<TreeNode> left = dfs(l, i - 1);
            List<TreeNode> right = dfs(i + 1, r);
            
            for (TreeNode a : left) {
                for (TreeNode b : right) {
                    TreeNode root = new TreeNode(i);
                    root.left = a;
                    root.right = b;
                    res.add(root);
                }
            }
        }
        return res;
    }
    public List<TreeNode> generateTrees(int n) {
        return dfs(1, n);
    }
}
```

