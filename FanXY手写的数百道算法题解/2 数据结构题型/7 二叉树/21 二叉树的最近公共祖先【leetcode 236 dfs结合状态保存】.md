# $$\color{Red}{二叉树的最近公共祖先【leetcode 236 dfs结合状态保存】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

 

**提示：**

- 树中节点数目在范围 `[2, 105]` 内。
- `-109 <= Node.val <= 109`
- 所有 `Node.val` `互不相同` 。
- `p != q`
- `p` 和 `q` 均存在于给定的二叉树中。



## 解析

`dfs` 递归获取的状态会传递给外层的 `dfs` 那么我们可以当出现 `q` 或者 `p` 则记录状态，使用二进制记录,【00 : 都不存在】【01 : p存在 q不存在】【10 : p不存在 q存在】【01 : 都存在】

当出现 状态为 11 并且



```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    static TreeNode ans;
    static int dfs(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) return 0;
        int state = dfs(root.left, p, q);
        if (root == p) state |= 1;
        else if (root == q) state |= 2;
        state |= dfs(root.right, p, q);
        if (state == 3 && ans == null) ans = root;
        return state;
    }

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        ans = null;
        dfs(root, p, q);    
        return ans;
    }
}
```

