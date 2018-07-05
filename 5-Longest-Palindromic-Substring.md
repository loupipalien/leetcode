## Longest Palindromic Substring
给定字符串 s, 找到 s 中最长的回文子串; 你可以假定 s 的最大长度为 1000

#### 示例 1:
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案
```

#### 示例 2:
```
输入: "cbbd"
输出: "bb"
```

### 概述
此文适合于入门读者; 将会介绍以下概念: 回文, 动态编程, 字符串操作; 确保你理解什么是回文; 回文是指一个字符串从头或从尾读起来都是一样的; 例如, S = "aba" 是回文, S = "abc" 则不是

### 解法

#### 方法 1: 最长公共字串
##### 普遍错误
一些人想要想出快速的方法, 不幸的是存在缺陷 (然而其实很容易正确):
```
反转字符串 S 编程 S'; 找到 S 和 S' 的最长公共字串, 那就一定是最长的回文字符串了
```
这看起来是奏效的, 现在让看以下一些示例  
例如: S = "caba", S' = "abca"; S 与 S' 之间的最长公共子串是 "aba", 这也是答案  
在尝试另外一个示例: S = "abacdfgdcaba", S' = "abacdgfdcaba"; S 与 S' 之间的最长公共子串是 "abacd"; 显然这不是有效的回文

##### 算法
当存在一个反序的非回文子串的拷贝是 S 的一部分时, 我们可以看到最长公共子串方法失败; 为了纠正这一点, 每次我们找到一个最长公共子串候选时, 我们去检查子串的索引是否与反序子串原始索引相同; 如果是, 我们尝试更新发现的回文; 如果不是, 我们跳过并查找下一个候选  
本文给我们了一个 $O(n^2)$ 时间复杂度的动态规划解法, 此解法会用 $O(n^2)$ 个空间 (可以被提升为只使用 $O(n)$ 个空间); 请在[这里] (https://en.wikipedia.org/wiki/Longest_common_substring) 阅读了解更多关于最长公共子串

#### 方法 2: 蛮力
显而易见, 蛮力解法是选出所有可能的子串起始和结束位置, 并校验其是否为回文
##### 复杂度分析
- 时间复杂度: $O(n^3)$; 假定 N 为输入字符串长, 则总共有 $\frac {n(n - 1)} {2}$ 个子串 (包括平凡解, 即一个字符本身也是一个回文); 因为验证每个子串会花费 $O(n)$ 时间, 运行时间复杂度是 $O(n^3)$
- 空间复杂度: $O(1)$

#### 方法 3: 动态规划
为了提升蛮力解法, 我们首先观察到当校验回文时我们可以如何避免不必要的重复计算; 考虑 "ababa", 如果我们已经知道 "bab" 是一个回文, 那么显然 "ababa" 一定是个回文, 因为左右两个结束字符相等; 我们定义 $P(i,j)$ 如下
$$ P(i,j) = \begin{cases} true, & \text {如果子串 S_i ... S_j 是一个回文} \\ false, & \text {其他} \end{cases} $$
因此
$$ P(i,j) = (P(i+1, j-1) 和 S_i == S_j) $$
基本示例是
$$ P(i,j) = true \\ P(i,i+1) = (S_i == S_{i+1}) $$
这样产生了一个直接的 DP 解法, 一开始我们初始化一个和两个字符作为回文, 按照我们的方式查找直到所有三个字符的回文, 等等
##### 复杂度分析
- 时间复杂度: $O(n^2)$, 此解法的运行复杂度是 $O(n^2)$
- 空间复杂度: $O(n^2)$, 会使用 $O(n^2)$ 的空间存储表
##### 附加练习
能否提升以上的空间复杂度以及如何提升?

#### 方法 4: 从中心扩展
事实上, 我们可以只使用常量空间在 $O(n^2)$ 的时间内解决它  
我们观察到回文是关于自己中心对称的; 因此, 一个回文必然可以从它的中心扩展开来, 并且这里只有 2n -1 个中心  
你也许会问为什么这里有 2n - 1 而不是 n 个中心? 原因是一个回文的中心可以是两个字符中间, 因此回文有偶数个字符时 (例如 "abba"), 它的中心在两个 "b" 之间
```
public String longestPalindrome(String s) {
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
```
##### 复杂度分析
- 时间复杂度: $O(n^2)$, 因为从中心扩展一个回文需要花费 $O(n)$ 的时间, 总体复杂度是 $O(n^2)$
- 空间复杂度: $O(1)$

#### 方法 5: 马拉车算法
这里有一个时间复杂度为 $O(n)$ 的马拉车算法, [这里](http://articles.leetcode.com/longest-palindromic-substring-part-ii/) 有详细说明; 然而它是一个非平凡解, 并且没人期望你能在 45 分钟的编码过程中想出这个算法; 但是, 请去理解它, 我保证这会很有趣

>**参考:**
[Longest Palindromic Substring](https://leetcode.com/articles/longest-palindromic-substring/)
