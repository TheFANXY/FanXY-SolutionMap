# <font color='red'>最小路径和【leetcode64 摘花生模板】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

### 题目说明

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`



### 解析

这题比起摘花生不同就是直接给了一个起点下标是从 0 开始的。

那么就很麻烦，需要自己处理边界问题，即出现第一行第一列判断上方和左方的越界，那么只能多写几个条件语句逐个判断了，没什么好办法。

可以直接在原数组上进行，因为仔细思考 `dp` 的顺序，更新一个点之前，左边和上边的点一定已经完成更新了。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;

        for (int i = 0; i < n; i ++) 
            for (int j = 0; j < m; j ++) {
                if (i == 0 && j == 0) continue;
                else if (j == 0) grid[i][j] = grid[i-1][j] + grid[i][j];
                else if (i == 0) grid[i][j] = grid[i][j-1] + grid[i][j];
                else grid[i][j] = Math.min(grid[i-1][j], grid[i][j-1]) + grid[i][j];
            }

        return grid[n-1][m-1];            
    }
}
```

