# <font color='bb000'>合并两个排序数组【leetcode 88 归并不开新】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matrix[i][j] <= 109`
- 每行的所有元素从左到右升序排列
- 每列的所有元素从上到下升序排列
- `-109 <= target <= 109`

### 解析

这里如果按照不优化的情况下使用二分，并不具备全局单调性，所以使用的话，需要一行一行使用，一列一列使用，这么下去复杂度太高。

而找出能舒服二分的方法，即维护一个矩形，上下左右四个界进行迭代二分，缩小区间，有点难写。

所以这里其实有一种取巧的方法，即利用单调性。

当一个点的值 `t > target` 的情况下，那么它右边的所有行都不可能选取，因为右侧当行本身大于他，而下面的数也都大更应该于它，那么右下面区域中的点的值更是大于 `target` ，因此可以向左移动，删除这片区域。

当一个点的值 `t < target` 的情况下，那么它左侧的所有行都不可能选取，因为左侧当行本身小于他，而上面的数也都更应该小于它，那么左上面区域中的点的值更是小于 `target` ，因此可以向下移动，删除这片区域。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length, m = matrix[0].length;
        int i = 0, j = m - 1;
        while (i < n && j >= 0) {
            int t = matrix[i][j];
            if (t == target) return true;
            else if (t > target) j -= 1;
            else if (t < target) i += 1;
        }
        return false;
    }
}
```

