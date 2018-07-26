## Remove Duplicates from Sorted Array
给定一个有序数组 nums, 就地移除重复鞥等元素使得每个元素仅出现一次, 然后返回新的长度  
不允许分配额外的空间给另外的数组, 你能做的是通过使用 $O(1)$ 的额外内存就地修改输入数组

#### 示例 1:
```
给定 nums = [1, 1, 2]
你的函数应该返回长度 = 2, 并且前两个元素是 1 和 2; 超过你返回的长度的数组元素是什么不重要
```
#### 示例 2:
```
给定 nums = [0,0,1,1,1,2,2,3,3,4]
你的函数应该返回长度 = 2, 并且前五个元素是 0, 1, 2, 3, 4; 超过你返回的长度的数组元素是什么不重要
```

#### 说明
疑惑为什么返回值是一个整数但你的答案是一个数组?  
注意, 输入数组是通过引用传递, 这意味着改变输入数组将会被调用这知晓  
你可以这样认为
```
// nums 是通过引用传递 (不能做复制)
int len = removeDuplicates(nums);
// 你的函数对 nums 的任何修改都会被调用者知晓
// 使用你的函数返回的长度, 打印前 len 个元素
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

### 解法
#### 方法 1: 两个指针
##### 算法
因为数组已经是有序的了, 我们可以维护两个指针 i 和 j, i 是慢指针而 j 是快指针; 只要 nums[i] == nums[j], 我们就增加 j 跳过这个重复元素  
当我们遇到 nums[i] != nums[j], 即重复已经结束, 所以我们必须拷贝它的值到 nums[i + 1], i 然后自增; 我们重复这个过程直到 j 到达数组尾
```
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$, 假定数组的长度为 n, 遍历每个 i 和 j 至多需要 n 步
- 空间复杂度: $O(1)$

>**参考:**
[Remove Duplicates from Sorted Array](https://leetcode.com/articles/remove-duplicates-from-sorted-array/)
