## 2. Add Two Numbers
你有两个表示非负整数的非空链表; 这些数字是被反序存储的, 并且每个节点只包含一个数字; 将这两个数字相加并将其作为链表返回  
你可以假定两个数字不包含任何前导零, 除了数字 0 本身
#### 示例
```
输入: (2 -> 4 -> 3) + (5 -> 6 -> 4)
输出: 7 -> 0 -> 8
解释: 342 + 465 = 807
```

### 解法
#### 方法 1: 基础数学
##### 直觉
使用一个变量跟踪进位, 并且从列表头开始模拟逐位求和, 列头包含的是最低有效位
![Image](https://leetcode.com/articles/Figures/2_add_two_numbers.svg)
图 1: 两数相加的图示: 342 + 465 = 807; 每个节点包含一位数字并且这些数字是反序存储的

##### 算法
正如你如何在纸上计算两数之和, 我们从最低位开始相加, 即 l1 和 l2 的头; 因为每个数字的范围是 0 - 9, 两数相加可能会 "溢出"; 例如 5 + 7 = 12; 在此示例中, 我们将当前数字置为 2, 并且将 carry = 1 带入下一迭代; 进位必定是 0 或 1, 因为可能的最大两数之和 (包括进位) 是 9 + 9 + 1 = 19  
伪代码如下:  
- 初始化当前节点假定为返回列表的头
- 初始化进位为 0
- 初始化 p 和 q 分别为 l1 和 l2 的头节点
- 循环变量列表 l1 和 l2 直到各自的尾节点
  - 设置 x 为节点 p 的值; 如果 p 已经到达 l1 的尾节点, 则设置为 0
  - 设置 y 为节点 q 的值; 如果 q 已经到达 l2 的尾节点, 则设置为 0
  - 设置 sum = x + y + carry
  - 创建一个数值为 (sum mod 10) 的新节点, 并且将其设置为当前节点的后缀, 然后将当前节点推进到下一节点
  - 同时也推进 p 和 q 到下一节点
- 检查是否 carry = 1, 如果是则追加一个数值为 1 的节点到返回列表
- 返回假定头节点的后缀节点

注意, 我们使用了一个假定头节点简化了代码; 如果没有一个假定头节点, 你不得不写额外的条件语句去初始化头节点的值  
额外小心以下用例

|测试用例|说明|
|-|-|
|l1 = [0, 1], l2 = [0, 1, 2]|当一个列表比另一个长|
|l1 = [], l2 = [0, 1]|当一个列表为 null, 即意味着是一个空列表|
|l1 = [9, 9], l2 = [1]|在结尾和有额外的进位 1, 这个容易忘记的|

```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```
##### 复杂度分析
- 时间复杂度: $O(max(m, n))$; 假定 m 和 n 各为 l1 和 l2 的长度, 以上算法迭代最多为 max(m, n) 次
- 空间复杂度: $O(max(m, n))$; 新列表的长度最多是 max(m, n) + 1

##### 后续
如果链表中的数字是正序存储的又该如何? 例如
```
(3 -> 4 -> 2) + (4 -> 6 -> 5) = 8 -> 0 -> 7
```

>**参考:**
[Add Two Numbers](https://leetcode.com/articles/add-two-numbers/)
