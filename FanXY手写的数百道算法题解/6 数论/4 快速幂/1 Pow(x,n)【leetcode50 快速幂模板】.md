# <font color="bb000">Pow(x,n)【leetcode50 快速幂模板】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

实现 pow(*x*, *n*) ，即计算 `x` 的整数 `n` 次幂函数（即，`xn` ）。

 

**示例 1：**

```
输入：x = 2.00000, n = 10
输出：1024.00000
```

**示例 2：**

```
输入：x = 2.10000, n = 3
输出：9.26100
```

**示例 3：**

```
输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

 

**提示：**

- `-100.0 < x < 100.0`
- `-231 <= n <= 231-1`
- `n` 是一个整数
- 要么 `x` 不为零，要么 `n > 0` 。
- `-10^4 <= xn <= 10^4`



### 解析

求 `x ^ n` ，暴力做法，让 `x` 自乘 `n` 次。

二进制优化 =====> 让 `n`  进行二进制分堆。

### [多重背包:三种语言+(迭代版和物理版)二进制分堆](https://www.acwing.com/solution/content/190295/) 

我们把枚举` n `次 换成` log(n) `次，分堆为【这堆存在的前提是 `n` 对应的位数为 `1`】

**`x ^ (2^0)`**,**` x ^ (2^1)`** ...... **`x ^ (2^log(n))`**

而为了表示出我们的 **` x ^ n `** 只需要进行选取相乘，根据二进制组合必能表示出对应的数字

```java
class Solution {
    public double myPow(double x, int n) {
        boolean is_minus = n < 0;
        double res = 1;

        for (long k = Math.abs((long) n); k > 0; k >>= 1) {
            if ((k & 1) == 1) res *= x;
            x *= x;
        }
        if (is_minus) res = 1 / res;
        return res;
    }
}
```

