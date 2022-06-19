## 题目
[454 四数相加II](https://leetcode-cn.com/problems/4sum-ii/)
给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：
0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
## 思路
本题和leetcode 1基本一样，不同的是4数相加，解题思路是一样的。
本题解题步骤：

1. 首先定义 一个unordered_map，key放a和b两数之和，value 放a和b两数之和出现的次数。
1. 遍历nums1和num2数组，统计两个数组元素之和，和出现的次数，放到map中。
1. 定义int变量count，用来统计 a+b+c+d = 0 出现的次数。
1. 在遍历nums3和nums4数组，找到如果 0-(c+d) 在map中出现过的话，就用count把map中key对应的value也就是出现次数统计出来。
1. 最后返回统计值 count 就可以了
```cpp
class Solution
{
public:
    int fourSumCount(vector<int> &nums1, vector<int> &nums2, vector<int> &nums3, vector<int> &nums4)
    {
        unordered_map<int, int> map; // key:num1+num2, value:num1+num2出现次数
        for (auto n1 : nums1)
            for (auto n2 : nums2)
            {
                map[n1 + n2]++; //记录两数和的次数
            }
        int result = 0;
        for (auto n3 : nums3)
            for (auto n4 : nums4)
            {
                int sum = n3 + n4;
                if (map.find(0 - sum) != map.end())
                {
                    //找到说明四数和为0，map中有value次可以与n3+n4和为0
                    result += map[0 - sum];
                }
            }
        return result;
    }
};
```
