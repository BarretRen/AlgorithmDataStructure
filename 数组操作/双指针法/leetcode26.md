## 题目
[26 删除有序数组中的重复项]](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)
给你一个 升序排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 k 个元素，那么 nums 的前 k 个元素应该保存最终结果。
将最终结果插入 nums 的前 k 个位置后返回 k 。

输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
## 思路
首先注意数组是有序的，那么重复的元素一定会相邻。
考虑用**双指针法**，一个在前记作 p，一个在后记作 q，算法流程如下：

- 比较 p 和 q 位置的元素是否相等。
   - 如果相等，q 后移 1 位
   - 如果不相等，将 q 位置的元素复制到 p+1 位置上，p 后移一位，q 后移 1 位
- 重复上述过程，直到 q 等于数组长度。
- 返回 p + 1，即为新数组长度。

![](leetcode26.assets/1644940059505-99181b52-e57b-4849-964c-7cb6832901fb.png)
```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slowIndex = 0;
        for (int fastIndex = 1; fastIndex < nums.size(); fastIndex++) {
            if (nums[slowIndex] != nums[fastIndex]) {
                slowIndex++;
                nums[slowIndex] = nums[fastIndex];
            }
        }
        return slowIndex + 1;
    }
};
```
