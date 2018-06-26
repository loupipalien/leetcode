### 1. Two Sum
给定一个整形数组, 返回相加为指定值的两个数的下标; 你可以假定每个输入有且只有一个解, 但你不能使用一个元素两次
#### 示例
```
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
返回 [0, 1]
```

### 解法
#### 方法 1: 蛮力
蛮力方法是简单的, 遍历每个元素 `x` 并找到另一个值等于 `target - x` 即可
```
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[j] == target - nums[i]) {
                return new int[] { i, j };
            }
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
##### 复杂度分析
- 时间复杂度: $O(n^2)$; 对于每个元素, 我们试图循环遍历数组中其余元素找到补数, 这会花费 $O(n)$ 的时间; 因此时间复杂度是 $O(n^2)$
- 空间复杂度: $O(1)$

#### 方法 2: 双程哈希表
为了提高我们的运行时间复杂度, 我们需要一个更有效的方式去检查是否有补数存在于数组中; 如果补数存在, 我们需要查找它的下标; 那什么是维护数组中每个元素和它下标的映射的最好方法呢? 哈希表  
我们通过空间换速度的方式将查找时间从 $O(n)$ 减少到 $O(1)$; 哈希表的建立就是为了这个目的, 它支持近乎常量时间的查找; 我说 "几乎" 是因为如果有冲突发生, 查找会退化为 $O(n)$ 时间; 但只要谨慎的选择了哈希函数, 在哈希表中的查找就会分摊为 $O(1)$ 时间  
使用两次迭代简单实现; 在第一次迭代中, 我们将每个元素的值和它的小标添加到表中; 然后, 在第二次迭代中我们检查是否每个元素的补数 (`target - nums[i]`) 存在于表中; 注意补数必须不能是 nums[i] 本身
```
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        map.put(nums[i], i);
    }
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement) && map.get(complement) != i) {
            return new int[] { i, map.get(complement) };
        }
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$; 我们遍历了包含 n 个元素的列表两次; 因为哈希表将查找时间减少到 $O(1)$, 时间复杂度是 $O(n)$
- 空间复杂度: $O(n)$; 要求的额外空间依赖于存储在哈希表中的元素个数, 正好存储了 n 个元素

#### 方法 3: 单程哈希表
事实证明我们可以一次搞定; 当我们迭代并插入元素到表中时, 同时我们回顾检查是否当前元素的补数已经存在于表中; 如果存在, 我们可以找到一个解并立即返回
```
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```
##### 复杂度分析
- 时间复杂度: $O(n)$; 我们遍历了包含 n 个元素的列表一次; 每次在表中查找花费仅 $O(1)$ 时间
- 空间复杂度: $O(n)$; 要求的额外空间依赖于存储在哈希表中的元素个数, 正好存储了 n 个元素

>**参考:**
[Two Sum](https://leetcode.com/articles/two-sum/)
