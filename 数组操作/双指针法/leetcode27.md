## 题目
[27 移除元素](https://leetcode-cn.com/problems/remove-element/)
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。
不要使用额外的数组空间，你必须仅使用 $O(1)$ 额外空间并**原地**修改输入数组。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
示例 1: 给定 nums = [3,2,2,3], val = 3, 函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。 你不需要考虑数组中超出新长度后面的元素。
示例 2: 给定 nums = [0,1,2,2,3,0,4,2], val = 2, 函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
## 思路
由于题目要求删除数组中等于 val的元素，因此输出数组的长度一定小于等于输入数组的长度，我们可以把输出的数组直接写在输入数组上。
可以**使用双指针法**：快指针指向当前将要处理的元素，慢指针指向下一个将要赋值的位置。

- 如果快指针指向的元素不等于val，它一定是输出数组的一个元素，我们就将快指针指向的元素复制到慢指针位置，然后将快慢指针同时右移；
- 如果快指针指向的元素等于val，它不能在输出数组里，此时慢指针不动，快指针右移一位。

整个过程保持不变的性质是：区间`[0, slowIndex)`中的元素都不等于val。当两个指针遍历完输入数组以后，`showIndex`的值就是输出数组的长度。
这样的算法在最坏情况下（输入数组中没有元素等于val），左右指针各遍历了数组一次。
![](leetcode27.assets/1644934703661-60e8d363-5bb9-42c4-a9d1-09c019cac0bf.gif)
```cpp
// 时间复杂度：O(n)
// 空间复杂度：O(1)
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {
            if (val != nums[fastIndex]) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
};
```

