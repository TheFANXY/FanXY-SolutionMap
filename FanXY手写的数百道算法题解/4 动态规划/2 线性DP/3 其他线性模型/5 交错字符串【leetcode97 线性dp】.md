# <font color='bb000'>交错字符串【leetcode97 线性dp】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给定三个字符串 `s1`、`s2`、`s3`，请你帮忙验证 `s3` 是否是由 `s1` 和 `s2` **交错** 组成的。

两个字符串 `s` 和 `t` **交错** 的定义与过程如下，其中每个字符串都会被分割成若干 **非空** 子字符串：

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- **交错** 是 `s1 + t1 + s2 + t2 + s3 + t3 + ...` 或者 `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**注意：**`a + b` 意味着字符串 `a` 和 `b` 连接。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
输出：true
```

**示例 2：**

```
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
输出：false
```

**示例 3：**

```
输入：s1 = "", s2 = "", s3 = ""
输出：true
```

 

**提示：**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`、`s2`、和 `s3` 都由小写英文字母组成

 

**进阶：**您能否仅使用 `O(s2.length)` 额外的内存空间来解决它?



### 解析

一般两个字符串的匹配问题，除了爆搜，可以尝试使用 `DP` 进行解决。

根据 `DP` 分析法，设置状态方程 `f[i][j]` 表示从 `s1[0, i]` 的部分，和 `s2[0, j]` 的部分，能否转化为 `s3` 。对于状态划分，以最后一位字符 `s3[i + j]` 这位字符到底是 `s1[i]` 还是 `s2[j]` 进行划分。

那么其实能想到，如果 `s1[i] == s3[i + j]` 那么需要状态转移自 `f[i - 1][j]` ，需要满足这两段能满足为 `true` 的情况下，才能保证 `f[i][j]` 为 `true` ，`s2[j] == s3[i + j]` 同理。

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int n = s1.length(), m = s2.length();
        if (s3.length() != n + m) return false;
        s1 = " " + s1;
        s2 = " " + s2;
        s3 = " " + s3;
        boolean [][] f = new boolean [n + 1][m + 1];
        for (int i = 0; i <= n; i ++) 
            for (int j = 0; j <= m; j ++) {
                if (i == 0 && j == 0) f[i][j] = true;
                else {
                    if (i != 0 && s1.charAt(i) == s3.charAt(i + j)) f[i][j] = f[i - 1][j];
                    if (j != 0 && s2.charAt(j) == s3.charAt(i + j)) f[i][j] = f[i][j] || f[i][j - 1];
                } 
            }

        return f[n][m];            
    }
}
```

