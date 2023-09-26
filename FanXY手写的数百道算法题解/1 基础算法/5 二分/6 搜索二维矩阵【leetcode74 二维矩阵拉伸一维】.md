# <font color='red'>搜索二维矩阵【leetcode74 二维矩阵拉伸一维】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

# 题目介绍

给你一个满足下述两条属性的 `m x n` 整数矩阵：

- 每行中的整数从左到右按非递减顺序排列。
- 每行的第一个整数大于前一行的最后一个整数。

给你一个整数 `target` ，如果 `target` 在矩阵中，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/mat2.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`



### 解析

这道题其实题目的意思就是，每行从左到右递增，每列从上到下递增，且每行末尾元素必小于下一行第一个元素，即按正常遍历顺序，整个数组递增。

那么搜索一个数，还是递增，明显的不能再明显了，二分。

只需要进行一下一维位置转换二维，显然直接就是 `row = cnt / m` ，`col = cnt % m` 。

题毕，简单题。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int n = matrix.length, m = matrix[0].length;
        int l = 0, r = n * m - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (matrix[mid / m][mid % m] < target) l = mid + 1;
            else r = mid;
        }
        return matrix[l / m][l % m] == target;
    }
}
```

