## ZigZag Conversion
将字符串 "PAYPALISHIRING" 在给定的行数上用之字形写出, 形如: (为了更好的可读性, 你可能想用固定字体展示这种模式)
```
P   A   H   N
A P L S I I G
Y   I   R
```
然后逐行读即: "PAHNAPLSIIGYIR"  
代码使用一个字符串和做出转换的给定行数
```
string convert(string s, int numRows);
```

#### 示例 1:
```
输入: s = "PAYPALISHIRING", numRows = 3
输出: "PAHNAPLSIIGYIR"
```
#### 示例 2:
```
输入: s = "PAYPALISHIRING", numRows = 4
输出: "PINALSIGYAHRPI"
说明:

P     I    N
A   L S  I G
Y A   H R
P     I
```

### 解法
#### 方法 1: 按行排序
##### 直觉
通过从左到右迭代遍历字符串, 我们可以很容易的确定一个字符属于之字形中的哪一行

##### 算法
我们可以用 min(numRows, len(s)) 个列表表示之字形的非空行; 从左向右迭代遍历字符串 s, 将每个字符追加到正确的行中; 正确的行可以使用两个变量来追踪: 当前行和当前方向  
当前方法改变仅发生在当我们移动到了最顶行或移动到了最低行
```
class Solution {
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        List<StringBuilder> rows = new ArrayList<>();
        for (int i = 0; i < Math.min(numRows, s.length()); i++)
            rows.add(new StringBuilder());

        int curRow = 0;
        boolean goingDown = false;

        for (char c : s.toCharArray()) {
            rows.get(curRow).append(c);
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        StringBuilder ret = new StringBuilder();
        for (StringBuilder row : rows) ret.append(row);
        return ret.toString();
    }
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 其中 n == len(s)
- 空间复杂度: $O(n)$

#### 方法 2: 按行遍历
如逐行阅读之字形的顺序遍历字符
##### 算法
从第 0 行开始遍历, 然后第 1 行, 然后第 2 行, 等等...  
对于所有整数 k:
- 第 0 行中的字符位于下标 k * (2 * numRows - 2) 的位置
- 在第 numRows - 1 行中的字符位于下标 k * (2 * numRows - 2) + numRows -1 的位置
- 对于中间第 i 行中的字符位于下标 k * (2 * numRows − 2) + i 和 (k + 1) * (2 * numRows - 1) - i 的位置
```
class Solution {
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        StringBuilder ret = new StringBuilder();
        int n = s.length();
        int cycleLen = 2 * numRows - 2;

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j + i < n; j += cycleLen) {
                ret.append(s.charAt(j + i));
                if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                    ret.append(s.charAt(j + cycleLen - i));
            }
        }
        return ret.toString();
    }
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 其中 n == len(s), 每个下标只被遍历一次
- 空间复杂度: $O(n)$, 对于 cpp 实现如果返回的字符串不被认为是额外的空间, 则空间复杂度是 $O(1)$

>**参考:**
[ZigZag Conversion](https://leetcode.com/articles/zigzag-conversion/)
