## 题目
[239 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/submissions/)
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
返回 滑动窗口中的最大值 。

示例 1：
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
```
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
## 思路
### 单调队列
首先能想到使用先进先出队列来表示k大小的滑动窗口区间，这样便于向右移动（队列pop一次，再push back一次）。
然后在分析，队列里的元素一定是要排序的，而且要最大值放在出队口，要不然怎么知道最大值呢。但如果把窗口里的元素都放进队列里，窗口移动的时候，队列需要弹出元素。那么问题来了，已经排序之后的队列顺序已经变了，怎么保证pop的是滑动窗口移出的值呢？

**其实队列没有必要维护窗口里的所有元素，只需要维护有可能成为窗口里最大值的元素就可以了，同时保证队里里的元素数值是由大到小的。**
那么这个维护元素单调递减的队列就叫做**单调队列，即单调递减或单调递增的队列。C++中没有直接支持单调队列，需要我们自己来一个单调队列**。

算法过程如下：
![](单调队列与leetcode239.assets/1645636669606-4007b930-6c1a-4d3a-86ba-6508a97c1b73.gif)
```cpp
class Solution
{
private:
    class MyQueue
    { //单调队列（从大到小）
    private:
        deque<int> que; // 使用deque来实现单调队列
    public:
        // 如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
        // 这样就保持了队列里的数值是从大到小的了。这样既对K区间内的值进行了比较，又保证了que里的值不会太多，不然比较会太耗时
        void push(int value)
        {
            while (!que.empty() && value > que.back())
            {
                que.pop_back();
            }
            que.push_back(value);
        }
        // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等就弹出。
        // 这样做的目的是仅把K区间外的值给去掉，其他值不变
        void pop(int value)
        {
            if (!que.empty() && value == que.front())
            {
                que.pop_front();
            }
        }
        // 查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
        int front()
        {
            return que.front();
        }
    };

public:
    vector<int> maxSlidingWindow(vector<int> &nums, int k)
    {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++)
        { // 先将前k的元素放进队列
            que.push(nums[i]);
        }
        result.push_back(que.front()); // result 记录前k的元素的最大值

        for (int i = k; i < nums.size(); i++)
        {
            que.pop(nums[i - k]);          // 滑动窗口移除最前面元素
            que.push(nums[i]);             // 滑动窗口前加入最后面的元素
            result.push_back(que.front()); // 记录对应的最大值
        }
        return result;
    }
};
```
### 优先级队列
上面的解法需要我们自己实现一个能自动排序的队列，而C++已经有一个**优先级队列**（priority_queue）**可以实现自动排序**，只不过优先级队列内部是一个二叉树，上面的单调队列是一个队列。

对于本题而言，初始时，我们将数组nums的前k个元素放入优先队列中。每当我们向右移动窗口时，我们就可以把一个新的元素放入优先队列中，此时**堆顶的元素就是堆中所有元素的最大值**。
然而这个最大值可能并不在滑动窗口中，在这种情况下，这个值在数组nums中的位置出现在滑动窗口左边界的左侧。因此，当我们后续继续向右移动窗口时，需要将其永久地从优先队列中移除。

我们不断地移除堆顶的元素，直到其确实出现在滑动窗口中。此时，堆顶元素就是滑动窗口中的最大值。为了方便判断堆顶元素与滑动窗口的位置关系，我们可以在优先队列中存储二元组(num,index)，表示元素num 在数组中的下标为index。
```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        priority_queue<pair<int, int>> q; //<value, index>
        for (int i = 0; i < k; ++i) {
            q.emplace(nums[i], i);
        }
        vector<int> ans = {q.top().first};//记录前k的元素的最大值
        
        for (int i = k; i < n; ++i) {
            q.emplace(nums[i], i);
            while (q.top().second <= (i - k)) {
                q.pop();//判断index不在滑动窗口的范围内了，移除该元素
            }
            ans.push_back(q.top().first);
        }
        return ans;
    }
};
```
