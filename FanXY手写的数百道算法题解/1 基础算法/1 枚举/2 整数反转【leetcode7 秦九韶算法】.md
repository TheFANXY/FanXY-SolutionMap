# <font color='bb000'>整数反转【leetcode7 秦九韶算法】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 



给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−2^31, 2^31 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

 

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

**示例 3：**

```
输入：x = 120
输出：21
```

**示例 4：**

```
输入：x = 0
输出：0
```

 

**提示：**

- `-2^31 <= x <= 2^31 - 1`



### 方法一 ：使用 long 【但题目要求似乎不允许使用】

直接使用秦九韶算法，而答案使用 `long` 类型，如果超过`int`范围，就返回0

```java
class Solution {
    public int reverse(int x) {
        long r = 0;
        while (x != 0) {
            r = r * 10 + x % 10;
            x /= 10;
        }
        if (r > Integer.MAX_VALUE || r < Integer.MIN_VALUE) return 0;
        return (int) r;
    }
}
```

### 方法二 ：移项锁定范围

当为正数时，出现越过  `2 ^ 32 - 1` 的情况。此时的 `r`满足：

```java
Integer.MAX_VALUE < r * 10 + x % 10
-----> r > (Integer.MAX_VALUE - x % 10) / 10
```

当为负数时，出现越过  `- 2 ^ 32` 的情况。此时的 `r`满足：

```java
Integer.MIN_VALUE > r * 10 + x % 10
-----> r < (Integer.MIN_VALUE - x % 10) / 10
```

综上，通过 `r` 的范围来判断越界。

```java
class Solution {
    public int reverse(int x) {
        int r = 0;
        while (x != 0) {
            if (r > 0 && r > (Integer.MAX_VALUE - x % 10) / 10) return 0;
            if (r < 0 && r < (Integer.MIN_VALUE - x % 10) / 10) return 0;
            r = r * 10 + x % 10;
            x /= 10;
        }
        return r;
    }
}
```





