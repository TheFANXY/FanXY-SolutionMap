# $$\color{Red}{二叉树的最小层数【leetcode111 叶子节点】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

**示例 2：**

```
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

 

**提示：**

- 树中节点数的范围在 `[0, 10^5]` 内
- `-1000 <= Node.val <= 1000`



### 解析

这道题简单想一下，看似涉及到层数，但是其实深搜是可以的【求点数并非求层数，广搜反而麻烦多了】，`bfs` 搜可以做到遍历到答案直接跳出，而 `dfs` 需要完全搜完左右子树，并取最小值。

而什么情况下可以返回？

这道题有一个很绕的地方，就是叶子节点是左右子树都为空的节点才是叶子节点。

当出现一个空另一个不空的情况下，它不是叶子节点，需要继续搜它不空的子节点，只有均空的情况下，才能直接返回答案。

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
    static int dfs(TreeNode root, int cnt) {
        if (root == null) return cnt;
        if (root.left == null) return dfs(root.right, cnt + 1);
        else if (root.right == null) return dfs(root.left, cnt + 1);
        else return Math.min(dfs(root.left, cnt + 1), dfs(root.right, cnt + 1));
    }   
    
    public int minDepth(TreeNode root) {
        return dfs(root, 0);
    }
}
```

