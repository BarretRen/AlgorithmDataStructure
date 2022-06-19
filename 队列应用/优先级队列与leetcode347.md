## 题目
[347 前K个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按任意顺序返回答案。

示例 1:
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
## 思路
这道题目主要涉及到如下三块内容：

1. 要统计元素出现频率：map<value, counts>统计
1. 对频率排序: 优先级队列
1. 找出前K个高频元素
### 什么是优先级队列
优先级队列**就是一个披着队列外衣的堆**，因为优先级队列对外接口只是从队头取元素，从队尾添加元素，再无其他取元素的方式，看起来就是一个队列。
优先级队列内部元素是自动依照元素的权值排列。缺省情况下priority_queue利用max-heap（大顶堆）完成对元素的排序，这个大顶堆是以vector为表现形式的complete binary tree（完全二叉树）。

**堆是一棵完全二叉树，树中每个结点的值都不小于（或不大于）其左右孩子的值。** 如果父亲结点是大于等于左右孩子就是大顶堆，小于等于左右孩子就是小顶堆。如果懒得自己实现的话，就直接用priority_queue（优先级队列）就可以了，底层实现都是一样的，从小到大排就是小顶堆，从大到小排就是大顶堆。
### 本题应用
本题我们要用小顶堆，**因为要统计最大前k个元素，只有小顶堆每次将最小的元素弹出**，最后小顶堆里积累的才是前k个最大元素。
```cpp
// 时间复杂度：O(nlogk)
// 空间复杂度：O(n)
class Solution
{
public:
    vector<int> topKFrequent(vector<int> &nums, int k)
    {
        // 统计元素出现频率
        unordered_map<int, int> map; // <value, counts>
        for (auto num : nums)
        {
            map[num]++;
        }

        // 对频率排序
        // 定义一个小顶堆，大小为k
        auto cmp = [](const pair<int, int> &lhs, const pair<int, int> &rhs)
        {
            return lhs.second > rhs.second;
        };
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> que(cmp);

        // 用固定大小为k的小顶堆，扫面所有频率的数值
        for (auto m : map)
        {
            que.push({m.first, m.second});
            if (que.size() > k)
            { // 如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                que.pop();
            }
        }

        // 找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--)
        {
            result[i] = que.top().first; //获取value
            que.pop();
        }
        return result;
    }
};
```
