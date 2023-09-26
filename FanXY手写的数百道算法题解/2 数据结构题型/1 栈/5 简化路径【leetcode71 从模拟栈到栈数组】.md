# <font color="bb000">简化路径【leetcode71 从模拟栈到栈数组】</font>

## [我的网站=> 分享了我关于前后端的各种知识和生活美食~](https://www.fanxy.cloud)

## [我于Acwing平台分享的零散刷的各种各样的题](https://www.acwing.com/blog/content/33005/) 

给你一个字符串 `path` ，表示指向某一文件或目录的 Unix 风格 **绝对路径** （以 `'/'` 开头），请你将其转化为更加简洁的规范路径。

在 Unix 风格的文件系统中，一个点（`.`）表示当前目录本身；此外，两个点 （`..`） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。任意多个连续的斜杠（即，`'//'`）都被视为单个斜杠 `'/'` 。 对于此问题，任何其他格式的点（例如，`'...'`）均被视为文件/目录名称。

请注意，返回的 **规范路径** 必须遵循下述格式：

- 始终以斜杠 `'/'` 开头。
- 两个目录名之间必须只有一个斜杠 `'/'` 。
- 最后一个目录名（如果存在）**不能** 以 `'/'` 结尾。
- 此外，路径仅包含从根目录到目标文件或目录的路径上的目录（即，不含 `'.'` 或 `'..'`）。

返回简化后得到的 **规范路径** 。

 

**示例 1：**

```
输入：path = "/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。 
```

**示例 2：**

```
输入：path = "/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根目录是你可以到达的最高级。
```

**示例 3：**

```
输入：path = "/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
```

**示例 4：**

```
输入：path = "/a/./b/../../c/"
输出："/c"
```

 

**提示：**

- `1 <= path.length <= 3000`
- `path` 由英文字母，数字，`'.'`，`'/'` 或 `'_'` 组成。
- `path` 是一个有效的 Unix 风格绝对路径。



### 解析

这道题一个解法是来自我们直接模拟整个过程，把所有边界情况都考虑到，难点在于怎么划分不同的情况。

当然这道题还看到了别人的`java` 的更简便做法 利用 `split` 震撼了我脆弱的心灵。🤡

#### 首先是模拟法

这个解法来自正常的思维，在各个文件的跳转，其实最开始可能会想成类似图论的节点跳跃，但是仔细思考，只会在当前层和下一层转换，那很有可能就是使用栈了。

使用栈，有个问题是，如果是一个一个字符进出栈，为了应对各种情况，岂不是每次都得不断从栈里面找寻上个为 `/` 的元素是否存在，然后字符要不停的进出栈，进行不止栈顶的别的位置的多重判断，这样显然会很繁琐，如果能每次只判断两个 `/` 之间的字符的进出栈，岂不美哉？🤡这就是第二个简便方法的初衷。

所以我们直接使用字符串 `StringBuilder` 模拟栈。其实 `String` 也一样，但是前者底层是可以直接拼接， `String` 需要反复 `new` 对象耗时代替，然后用一个 `String 类型的 对象 name` 记录文件名。

每当遍历字符不为 `/` 就不断的加入 `name` 中， 当出现 `/` ，就只需要判断一下，`name == ..` 就直接把答案已经记录的文件名和它的路径 `/` 都去除【这个是因为每当找到一个合法的文件路径，是直接带 `/` 一起加入答案的，所以这波需要去除上一层文件的 `/` 目录】

当为 `.` 或者为空的情况下，直接不加入即可，无需做任何行为，把 `name` 重置一下就行。

其他情况就是合法情况了，加入答案 `/文件名`。

这种情况下，如果出现答案是 `/` 的情况，按我们的思路，会变成空串，因为我们只有合法的文件名才会加入答案 `/文件名` 到答案字符串结尾。

所以为空的时候直接加一个 `/` 到答案即可。

```java
class Solution {
    public String simplifyPath(String path) {
        StringBuilder res = new StringBuilder();
        String name = "";
        if (path.charAt(path.length() - 1) != '/') path += '/';

        for (int i = 0; i < path.length(); i ++) {
            char c = path.charAt(i);
            if (c != '/') name += c;
            else {
                if (name.equals("..")) {
                    while (res.length() != 0 && res.charAt(res.length() - 1) != '/') 
                        res.deleteCharAt(res.length() - 1);
                    if (res.length() != 0) res.deleteCharAt(res.length() - 1);
                }
                else if (!name.equals(".") && !name.equals("")) {
                    res.append('/').append(name);
                }
                name = "";
            }
        }
        if (res.length() == 0) res.append('/');
        return res.toString();
    }
}
```



#### java的`String.split`和字符串`String.join`

我们上面的操作想达到的结果，其实就是通过遍历到 `/` 作为一个分解线区分，但是其实通过字符串的 `String.split` 就可以指定分隔符切割为字符串数组，这个大家肯定熟悉，因为用 `BufferedReader` 的时候就需要进行这种读入划分。

但是 `String.join` 可以把一个字符串数组，或者是字符串集合里面的元素，通过指定的分隔符连接成一个字符串，这个方法几乎没用到过，使用这个，再加上一个集合栈存放 `String` 类型，这道题就极其简单的解答了。但是复杂度确实高了很多。

```java
class Solution {
    public String simplifyPath(String path) {
        List <String> list = Arrays.asList("", ".", "..");
        LinkedList <String> q = new LinkedList<>();

        for (String s : path.split("/")) {
            if (!list.contains(s)) {
                q.add(s);
            } else if (!q.isEmpty() && s.equals("..")) {
                q.remove(q.size() - 1);
            }
        }

        return "/" + String.join("/", q);
    }
}
```











