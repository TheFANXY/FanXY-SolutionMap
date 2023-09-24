# <font color='bb000'>文本左右对齐【leetcode68 双指针模拟(困难)】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

### 题目介绍

给定一个单词数组 `words` 和一个长度 `maxWidth` ，重新排版单词，使其成为每行恰好有 `maxWidth` 个字符，且左右两端对齐的文本。

你应该使用 “**贪心算法**” 来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 `' '` 填充，使得每行恰好有 *maxWidth* 个字符。

要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。

文本的最后一行应为左对齐，且单词之间不插入**额外的**空格。

**注意:**

- 单词是指由非空格字符组成的字符序列。
- 每个单词的长度大于 0，小于等于 `maxWidth`。
- 输入单词数组 `words` 至少包含一个单词。

 

**示例 1:**

```
输入: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
输出:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**示例 2:**

```
输入:words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
输出:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
解释: 注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
     因为最后一行应为左对齐，而不是左右两端对齐。       
     第二行同样为左对齐，这是因为这行只包含一个单词。
```

**示例 3:**

```
输入:words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"]，maxWidth = 20
输出:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

 

**提示:**

- `1 <= words.length <= 300`
- `1 <= words[i].length <= 20`
- `words[i]` 由小写英文字母和符号组成
- `1 <= maxWidth <= 100`
- `words[i].length <= maxWidth`



### 解析🤡

这道题，初看就头皮发麻。感觉会有非常多的边界条件，不过还是下意识先思考到，为了先获取一行选多少单词，只能先通过第一个单词不带空格，后面的都自带一个空格，看看最边界【空格长度为1】的情况下，能否容纳，不能容纳的情况下，再判断这一行空格为多少。

为了满足这个能反悔先添加一个空格的行为，显然很适合用 **双指针。**

其实这道题真的把边界拆分之后，可以想到`大的划分下其实就两种情况。`

1. 左对齐
2. 左右对齐



#### 左对齐

正常情况下都是左右对齐，而最后一行和当只有一个单词占一行的情况下，需要左对齐，即先把单词按一个空格的间距放满，然后把后面的空位【如果能存在空位】补满空格。不存在其他边界条件了。



#### 左右对齐

当采取左右对齐的情况下，假设我们已知可分配空格数量为 `r` ，而这一行所有单词加起来的长度为 `len`【事实上之前双指针操作肯定多加了空格，为了先以最边界情况判断要放多少个单词，但可以直接通过 `j - 1 - i` 算出空隙数量，用之前的`len` 减去这个值，就把之前多加的空格给删除了】。

这种情况下，显然，为了均分放空格，先用整除判断每个空隙能拿到的基本空格数量。 即 `r / cnt`

而事实上，像上面题目说的一样，不能整除的情况下，多出来的空格就依次给前面的空隙优先一个空隙分配一个，这个能分配的空隙数量为 `r % cnt`

那么就前 `r % cnt` 个空隙分配 `(r / cnt) + 1` 个空格，后面的空隙分配 `r / cnt` 个即可。

结束~



### 题外话🎁

细节还是看代码，这道题看似很难，但是其实只需要把大情况划分，分治考虑不同的情况，然后把边界考虑完全，其实困难题，拆分后可能就是多个中等或者简单题的组合，所以很多时候边界问题太多，那就试试能不能划分为完全独立的几种情况，分别考虑各自内部的细节。🧙‍♂️

```java
class Solution {

    static StringBuilder get(int x) {
        StringBuilder res = new StringBuilder();
        while (x -- > 0) res.append(" ");
        return res;
    }

    public List<String> fullJustify(String[] words, int maxWidth) {
        int n = words.length;
        List<String> ans = new ArrayList<>();

        for (int i = 0; i < n; i ++) {
            int len = words[i].length();
            int j = i + 1;
            StringBuilder line = new StringBuilder();

            while (j < n && len + 1 + words[j].length() <= maxWidth) 
                len = len + 1 + words[j++].length();

            // 当只有一个单词 或者最后一行的情况下 左对齐后续补空格    
            if (j == n || j == i + 1) {
                line.append(words[i]);
                for (int k = i + 1; k < j; k ++) line.append(" ").append(words[k]);
                // 补充剩余空格
                while (line.length() < maxWidth) line.append(" ");
            }
            // 其余情况 左右对齐
            else {
                int cnt = j - 1 - i; // 空隙数量
                int r = maxWidth - (len - cnt); // 可分配空格数量
                int a = r % cnt; // 可多余分配一格的位置数量
                int b = r / cnt; // 正常分配空格数量
                line.append(words[i]);

                for (int k = i + 1; k < j; k ++) {
                    if (a -- > 0) line.append(get(b + 1)).append(words[k]);
                    else line.append(get(b)).append(words[k]);
                }
            }
            i = j - 1;
            ans.add(line.toString());
        }
        return ans;
    }
}
```

