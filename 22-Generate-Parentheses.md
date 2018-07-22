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
