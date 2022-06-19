## 题目
[209 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。
示例：
输入：s = 7, nums = [2,3,1,2,4,3] 输出：2 解释：子数组 [4,3] 是该条件下的长度最小的子数组。
## 思路
使用滑动窗口，以题目中的示例来举例，s=7， 数组是 2，3，1，2，4，3，来看一下查找的过程：
![](leetcode209.assets/1645002989592-76435257-13ed-4118-acf2-6fae01d570a5-1655596999256.gif)
在本题中实现滑动窗口，主要确定如下三点：

- 窗口内是什么？-- 满足其和 ≥ s 的长度最小的 连续 子数组
- 如何移动窗口的起始位置？**如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）**
- 如何移动窗口的结束位置？窗口的结束位置就是遍历数组的指针，窗口的起始位置设置为数组的起始位置就可以了。

**解题的关键在于窗口的起始位置如何移动**
```cpp
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int result = INT32_MAX;
        int sum = 0; // 滑动窗口数值之和
        int i = 0; // 滑动窗口起始位置
        int subLength = 0; // 滑动窗口的长度
        for (int j = 0; j < nums.size(); j++) {
            sum += nums[j];
            // 注意这里使用while，每次更新 i（起始位置），并不断比较子序列是否符合条件
            while (sum >= s) {
                subLength = (j - i + 1); // 取子序列的长度
                result = result < subLength ? result : subLength;
                sum -= nums[i++]; // 这里体现出滑动窗口的精髓之处，不断变更i（子序列的起始位置）
            }
        }
        // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
        return result == INT32_MAX ? 0 : result;
    }
};

class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int length = nums.size();
        bool isFound = false;
        int left = 0;
        int sum = 0;
        for(int right = 0; right < nums.size(); right++)
        {
            sum +=nums[right];
            while(sum >= s)
            {
                isFound = true;
                length = (length > (right - left+1))? (right - left+1):length;
                sum -= nums[left++];
            }
        }
        return isFound ?length:0;
    }
};
```
