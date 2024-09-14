# 找到字符串中所有字符异位词【leetcode438 滑动窗口】

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.icu)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

# 题目介绍

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

 

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 

**提示:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母



### 字符排序进行异位词比较

这里利用字符串的一个性质取巧，可以轻松求解，但是复杂度高，因为需要一直不停的新建对象和排序。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int n = s.length();
        int m = p.length();
        List<Integer> ans = new ArrayList<>();
        if (m > n) return ans;
        char [] cp = p.toCharArray();
        Arrays.sort(cp);
        p = new String(cp);
        for (int i = 0; i + m - 1 < n; i ++) {
            String str = s.substring(i, i + m);
            char [] cs = str.toCharArray();
            Arrays.sort(cs);
            String x = new String(cs);
            if (x.equals(p)) ans.add(i);
        }
        return ans;
    }
}
```

### 滑动窗口

我们可以维护一个窗口，这个窗口内的字符，我们都会对此计数，假设我们还有一个哈希表，把模板串的字符都计数了。那么当我们出现两个哈希表匹配的字符完全相同【光数量相同不行】，同时计数也相同，那么此时证明窗口内的字符序列构成了异位词。

但是这么做很尴尬，我们难不成每移动一次窗口，就一个个数看看有没有对应字符，并统计数量相等？而且还需要保证不多不少，需要额外开变量。

### => 用一个哈希表进行数量抵消

1.  一旦变为0 说明匹配到了一个字符
2. 如果从0 变到 -1 或 1，说明失去匹配一个字符

这样我们就不需要逐个比较，通过统计完全匹配的字符数量，一旦和 p 的字符数量相等，即可完成。

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int [] cnt = new int [26];
        int needMatch = 0;
        for (int i = 0; i < p.length(); i ++) {
            if (cnt[p.charAt(i) - 'a'] == 0) needMatch ++;
            cnt[p.charAt(i) - 'a'] ++;
        }
        for (int L = 0, R = 0; R < s.length(); R ++) {
            // 右边移动 并更改匹配数量
            if (--cnt[s.charAt(R) - 'a'] == 0) needMatch --;
            // 左边也移动 更改匹配数量
            if (R - L + 1 > p.length()) {
                if (cnt[s.charAt(L) - 'a'] == 0) needMatch ++;
                cnt[s.charAt(L ++) - 'a'] ++;
            } 
            if (needMatch == 0) ans.add(L);
        }
        return ans;
    }
}
```

