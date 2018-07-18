## Remove Nth Node From End of List
给定一个链表, 移除从后往前第 n 个节点, 然后返回链表头节点

#### 示例:
```
给定的链表: 1->2->3->4->5, and n = 2.

移除从后往前第 2 个节点后, 链表变成 1->2->3->5.
```

#### 注意:
给定的 n 总是有效的

#### 附加:
你可以用一次遍历解出吗?

### 概述
这篇文章是针对入门者的, 介绍了以下思想: 链表反转和移除从后往前第 n 个节点

### 解法
#### 方法 1: 双程算法
##### 直觉
我们注意到此问题可以被简单的降解为另一个问题: 移除链表从前往后第 length - n 个节点, 即链表的长度是多少; 当我们知道了链表的长度则很容易解决这个问题
##### 算法
一开始我们加入一个辅助 "dummy" 节点, 它指向链表头节点; "dummy" 节点用于在链表只有一个节点时简化边界情况, 或者移除链表头节点; 在第一次遍历中, 我们知道了链表的长度; 然后我们设置一个指针指向 dummy 节点, 并且开始移动它遍历链表直到它到达第 length - n 个节点; 我们重新连接指向第 length - n 个节点的 next 指针, 然后我们就完成了
![Image](https://leetcode.com/media/original_images/19_Remove_nth_node_from_end_of_listA.png)
图 1. 从链表中移除第 L - n + 1 个元素

```
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    int length  = 0;
    ListNode first = head;
    while (first != null) {
        length++;
        first = first.next;
    }
    length -= n;
    first = dummy;
    while (length > 0) {
        length--;
        first = first.next;
    }
    first.next = first.next.next;
    return dummy.next;
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 算法对链表做了两次遍历, 第一次为了计算链表的长度, 第二遍为了找到节点; 这些操作的时间复杂度是 $O(n)$
- 空间复杂度: $O(1)$, 我们只用了常量的额外空间

#### 方法 2: 单程算法
##### 算法
以上算法可以被优化为单程; 不用一个指针, 我们可以用两个指针; 第一个从链表头开始一步步推进, 然后第二个指针再从链表头出发; 现在, 两个指针被部分节点分隔; 我们维护一个常量的间隔铜鼓同时推进两个指针直到第一个指针越过最后一个节点; 第二个指针将会指向链表从后往前的第 n + 1 个节点上; 我们重新将第二个指针指向的节点的 next 指针指向此节点的 next 的 next 节点
![Image](https://leetcode.com/media/original_images/19_Remove_nth_node_from_end_of_listB.png)
图 2. 从链表中移除第 L - n + 1 个元素

```
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    // Advances first pointer so that the gap between first and second is n nodes apart
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    // Move first to the end, maintaining the gap
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 此算法对链表只做了单程遍历, 因此时间复杂度是 $O(n)$
- 空间复杂度: $O(1)$, 我们只用了常量的额外空间

>**参考:**
[Remove Nth Node From End of List](https://leetcode.com/articles/remove-nth-node-from-end-of-list/)
