# <font color='bb000'>不同的子序列【leetcode115 线性dp】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你两个字符串 `s` 和 `t` ，统计并返回在 `s` 的 **子序列** 中 `t` 出现的个数，结果需要对 109 + 7 取模。

 

**示例 1：**

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

**示例 2：**

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```

 

**提示：**

- `1 <= s.length, t.length <= 1000`
- `s` 和 `t` 由英文字母组成





### 解析

这道题可以使用线性 `dp` ，划分属性为 `cnt`， 而状态为 `f[i][j]` 以 `s[1 -> i]` 可转为 `t[1 -> j]` 的数量。

对于字符串的线性 `dp` 一般以  **是否选择 `s[i]`** 作为划分依据。

即 若 `s[i]不选` 这种情况，即 `f[i][j] = f[i - 1][j]` 划分不选的状态方程。

若 `选择s[i]` 这种情况，肯定需要还得满足 `s[i] == t[j]` ，然后加上这个状态的数量 `f[i - 1][j - 1]` 。

```java
class Solution {
    public int numDistinct(String s, String t) {
        int n = s.length(), m = t.length();
        s = " " + s;
        t = " " + t;
        long [][] f = new long [n + 1][m + 1];
        for (int i = 0; i <= n; i ++) f[i][0] = 1;
        for (int i = 1; i <= n; i ++) {
            for (int j = 1; j <= m; j ++) {
                f[i][j] = f[i - 1][j];
                if (s.charAt(i) == t.charAt(j)) 
                    f[i][j] += f[i - 1][j - 1];
            }
        }
        return (int) f[n][m];
    }
}
```

