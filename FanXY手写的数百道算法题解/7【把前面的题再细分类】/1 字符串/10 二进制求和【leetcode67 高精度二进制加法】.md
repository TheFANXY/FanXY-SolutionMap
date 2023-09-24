# <font color='bb000'>二进制求和【leetcode67 高精度二进制加法】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你两个二进制字符串 `a` 和 `b` ，以二进制字符串的形式返回它们的和。

 

**示例 1：**

```
输入:a = "11", b = "1"
输出："100"
```

**示例 2：**

```
输入：a = "1010", b = "1011"
输出："10101"
```

 

**提示：**

- `1 <= a.length, b.length <= 104`
- `a` 和 `b` 仅由字符 `'0'` 或 `'1'` 组成
- 字符串如果不是 `"0"` ，就不含前导零



### 解析

这道题和高精度加法的区别，就是进位为`2`，高精度加法是 `10`，为什么写这个题解是因为高精度那道题没写，为了题解完整性就写了这道题。

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder x = new StringBuilder(a).reverse();
        StringBuilder y = new StringBuilder(b).reverse();
        StringBuilder ans = new StringBuilder();

        for (int i = 0, k = 0; i < x.length() || i < y.length() || k != 0; i ++) {
            if (i < x.length()) k += x.charAt(i) - '0';
            if (i < y.length()) k += y.charAt(i) - '0';
            ans.append(k % 2);
            k /= 2;
        }
        return ans.reverse().toString();
    }
}
```

