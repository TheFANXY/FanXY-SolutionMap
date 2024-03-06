# $$\color{Red}{从后序与中序遍历序列构造二叉树【leetcode106 双dfs融合】}$$

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例 2:**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

 

**提示:**

- `1 <= inorder.length <= 3000`
- `postorder.length == inorder.length`
- `-3000 <= inorder[i], postorder[i] <= 3000`
- `inorder` 和 `postorder` 都由 **不同** 的值组成
- `postorder` 中每一个值都在 `inorder` 中
- `inorder` **保证 **是树的中序遍历
- `postorder` **保证** 是树的后序遍历





### 解析

单从一个二叉树的后序遍历或者中序遍历能确定至少一种结构的二叉树，而如果能根据他们两个，能构造唯一的结构，题目保证肯定有解，所以不需要考虑无解的情况。

那么理论上，我们为了能完成从递归的角度从这些遍历数组中构造二叉树，首先思考，从后序遍历，我们无法知道左右子树，但是能确定根节点。而对于中序遍历，那么连根节点都确定不了，只能先从前序获得根节点，然后再带入中序遍历，我们神奇的发现，能获取当前根节点的左子树和右子树的中序遍历结构了。

那么我们把这个过程递归进行，对左子树和右子树，继续进行上述操作，即完成递归。

为了能每次取出对应根节点值的位置，可以预存一个哈希表，要不然每次都要遍历一下整个中序遍历数组。

假设我们从中序遍历获取根节点，在中序遍历的数组的位置为 `k` 。

那么递归进行的中序遍历【`inorder`】的左右子树的范围很简单就是 `[il, k - 1]` 和 `[k + 1, ir]`

但是后序遍历【`postorder`】我们只能锁定第一个根节点是最后面的元素即 `pr` 。

但是有一个关键信息，我们从中序遍历能获取左右子树的节点数量，即在后序遍历也需要满足这个限制，设在后序遍历下一层递归需要用到的左子树的右边界为 `x` ，那么肯定满足 `x - pl = k - 1 - il` ，那么很显然，这个左子树右边界为 `pl + k - 1 - il` 。

那么右子树右边界肯定是 `pr - 1` ，它的左边界就是左子树右边界的右边 即上面的一长串 `+ 1`。



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

    public static Map<Integer, Integer> map = new HashMap<>();

    public TreeNode build(int[] inorder, int[] postorder, int il, int ir, int pl, int pr) {
        if (il > ir) return null;
        TreeNode root = new TreeNode(postorder[pr]);
        int k = map.get(root.val);
        root.left = build(inorder, postorder, il, k - 1, pl, pl + k - 1 - il);
        root.right = build(inorder, postorder, k + 1, ir,pl + k - 1 - il + 1, pr - 1);
        return root;
    }

    public TreeNode buildTree(int[] inorder, int[] postorder) {
        map.clear();
        for (int i = 0; i < inorder.length; i ++) map.put(inorder[i], i);
        return build(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
    }
}
```

