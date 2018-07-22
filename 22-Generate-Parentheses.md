## Generate Parentheses
给定 n 对括号, 写一个可以生成所有符合规则的括号组合的函数

#### 示例:
```
输入: 3
输出: ["((()))","(()())","(())()","()(())","()()()"]
```

### 解法

#### 方法 1: 蛮力
##### 直觉
我们可以生成所有关于 "(" 和 ")" 字符的 $2^{2n}$ 个组合; 然后, 我们检查每一个是否是有效的
##### 算法
我们使用一个递归去生成所有序列; 所有 n 长度的序列正好是 "(" 加上所有 n - 1 长度的序列, 或者 ")" 加上 n - 1 长度的序列  
检查一个序列是否是有效的, 我们需要保持平衡, 即左括号的总数减去右括号的总数; 如果任何时候的结果小于 0, 或者最终的结果不等于 0, 那么序列就是无效的, 否则是有效的
```
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> combinations = new ArrayList();
        generateAll(new char[2 * n], 0, combinations);
        return combinations;
    }

    public void generateAll(char[] current, int pos, List<String> result) {
        if (pos == current.length) {
            if (valid(current))
                result.add(new String(current));
        } else {
            current[pos] = '(';
            generateAll(current, pos+1, result);
            current[pos] = ')';
            generateAll(current, pos+1, result);
        }
    }

    public boolean valid(char[] current) {
        int balance = 0;
        for (char c: current) {
            if (c == '(') balance++;
            else balance--;
            if (balance < 0) return false;
        }
        return (balance == 0);
    }
}
```
##### 复杂度分析
- 时间复杂度: $O(2^{2n}n)$, 对于 $2^{2n}$ 个序列, 我们需要创建和校验序列, 校验会花费 $O(n)$ 的时间
- 空间复杂度: $O(2^{2n}n)$, 假设每个序列都是有效的, 对于找到一个邻近届可见 方法 3

#### 方法 2: 回溯
##### 直觉和算法
与方法 1 中每次添加 '(' 或 ')' 不同, 仅在我们知道它仍能保持为有效序列时添加; 我们可以通过回溯我们之前已经追加了的左括号和右括号的总数计算得到  
如果还有剩余的 (共 n 个) 去替代, 我们可以追加一个左括号; 如果右括号的个数还没有超过左括号的个数, 我们可以追加一个右括号
```
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        backtrack(ans, "", 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, String cur, int open, int close, int max){
        if (cur.length() == max * 2) {
            ans.add(cur);
            return;
        }

        if (open < max)
            backtrack(ans, cur+"(", open+1, close, max);
        if (close < open)
            backtrack(ans, cur+")", open, close+1, max);
    }
}
```
##### 复杂度分析
我们的复杂度分析是基于理解在 `generateParenthesis(n)` 会生成多少个元素; 这个分析超出了本文的讨论范围, 但是它被证明是第 n 项的卡塔兰数 $\frac {1} {n + 1} (^{2n}_n)$, 它的渐进届是 $\frac {4^n} {n\sqrt{n}}$
- 时间复杂度: $O(\frac {4^n} {n\sqrt{n}})$, 每个有效的序列在回溯产生时至多需要 n 步
- 空间复杂度:  $O(\frac {4^n} {n\sqrt{n}})$, 正如上述所说, 使用 $O(n)$ 来存储序列

#### 方法 3: 闭合数
##### 直觉
TODO

>**参考:**
[Generate Parentheses](https://leetcode.com/articles/generate-parentheses/)
