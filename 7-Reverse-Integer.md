## Reverse Integer
给定一个 32 位的有符号整数, 反转整数的数位

#### 示例 1:
```
输入: 123
输出: 321
```
#### 示例 2:
```
输入: -123
输出: -321
```
#### 示例 3:
```
输入: 120
输出: 21
```
#### 注意

假定我们在某个只能存储范围在 $[-2^{31}, 2^{31} - 1]$ 的 32 位有符号整数的环境中工作; 为了这个问题的目的, 假定你的函数在反转整数后溢出时返回 0

### 解法
#### 方法 1: 出栈入栈数位 & 溢出前检查
##### 直觉
我们可以一次性构建出整数的数位反序; 在反序时, 在添加另一个数位时我们可以预先检查是否造成溢出
##### 算法
将一个整数反转类似于将字符串反转  
我们将重复的把 x 的数位取出, 然后推到 rev 的后面; 最后, rev 将会是 x 的反序  
在没有栈或列表的辅助下弹出和压入数位, 我们可以用数学计算
```
//弹出操作:
pop = x % 10;
x /= 10;

//压入操作:
temp = rev * 10 + pop;
rev = temp;
```
然而, 这个方法是危险的, 因为语句 temp = rev * 10 + pop 可能会造成溢出; 为了说明这个, 让我们假定 rev 是整数
1. 如果 $temp = rev * 10 + pop$, 那么 $rev \geq \frac {INTMAX} {10}$
2. 如果 $rev > \frac {INTMAX} {10}$, 那么 $temp = rev * 10 + pop$ 一定会溢出
3. 如果 $rev == \frac {INTMAX} {10}$, 当 $pop > 7$ 时 $temp = rev * 10 + pop$ 会溢出
相同的逻辑也适用当 rev 为负数时
```
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) return 0;
            if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
}
```
##### 复杂度分析
- 时间复杂度: O(log(x)), 近似等于 $\log_{10}(x)$
- 空间复杂度: $O(1)$

>**参考:**
[Reverse Integer](https://leetcode.com/articles/reverse-integer/)
