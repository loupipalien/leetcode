## Valid Anagram
给定两个字符串 s 和 t, 写一个确定 t 是否是 s 的异构词对函数

#### 示例 1:
```
输入: s = "anagram", t = "nagaram"
输出: true
```
#### 示例 2:
```
输入: s = "rat", t = "car"
输出: false
```
#### 注意
你可以假定字符串只包含小写字母

#### 后续
如果输入包含 unicode 字符呢? 对于这种情况你将如何调整你的解法

### 解法
#### 方法 1
##### 算法
将 `s` 的字母重新排序便是异位词 `t`; 因此, 如果 `t` 是 `s` 的异位词, 排序后的字符必将产生两个相同的字符串; 还有, 如果 `s` 和 `t` 有不同的长度, 则 `t` 一定不是 `s` 的异位词, 因此我们可以更早的返回
```
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    char[] str1 = s.toCharArray();
    char[] str2 = t.toCharArray();
    Arrays.sort(str1);
    Arrays.sort(str2);
    return Arrays.equals(str1, str2);
}
```
##### 复杂度分析
- 时间复杂度: $O(n\log n)$, 假定 n 是字符串 s 的长度, 排序耗时 $O(n\log n)$ 和比较两字符串耗时 $O(n)$; 排序时间是主要的, 所以时间复杂度是 $O(n\log n)$
- 空间复杂度: $O(1)$, 所需空间取决于排序的实现, 如果使用的是 `heapsort` 通常只需要 $O(1)$ 的辅助空间, `toCharArray()` 是将字符串做了一个拷贝所以也需要 $O(n)$ 的额外空间, 对于复杂度分析我们忽略了它是因为
  - 这是语言细节
  - 它依赖函数是如何设计的; 例如, 函数的参数类型可以修改为 `char[]`

#### 方法 2
##### 算法
为了校验 `t` 是否是 `s` 的异位词, 我们可以统计在两个字符串中每个字符出现的次数然后比较它们; 因为 `s` 和 `t` 仅包含 a - z, 一个 26 大小的统计表即可满足  
为了比较我们需要两个统计表吗? 实际上不需要, 因为我们可以对 `s` 中的每个字符做自增, 对 `t` 中的每个字符做自减, 然后检查统计是否归为 0 即可
```
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```
或者我们首先对 `s` 做自增统计, 然后对 `t` 做自减统计; 在任何点上统计小于 0, 我们即知道 `t` 中包含 `s` 中不包含的字符, 即可立刻返回
```
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] table = new int[26];
    for (int i = 0; i < s.length(); i++) {
        table[s.charAt(i) - 'a']++;
    }
    for (int i = 0; i < t.length(); i++) {
        table[t.charAt(i) - 'a']--;
        if (table[t.charAt(i) - 'a'] < 0) {
            return false;
        }
    }
    return true;
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 时间复杂度是 $O(n)$, 因为访问统计表是一个常量时间操作
- 空间复杂度: $O(1)$, 尽管我们使用了额外的空间, 空间复杂度是 $O(1)$ 是因为无论 n 多大表的大小是常量

#### 后续
如果输入包含 unicode 字符呢? 对于这种情况你将如何调整你的解法
#### 答案
使用哈希表而不是固定大小的统计表, 想象分配一个巨大的数组包含所有 unicode 的范围, 可能超过 [1 百万](https://stackoverflow.com/a/5928054/490463); 哈希表是一个更通用的解法并且能够适应任何范围的字符

>**参考:**
[Valid Anagram](https://leetcode.com/articles/valid-anagram/)
