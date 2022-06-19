## 题目
[997 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)
给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。
示例 1： 输入：nums = [-4,-1,0,3,10] 输出：[0,1,9,16,100] 解释：平方后，数组变为 [16,1,0,9,100]，排序后，数组变为 [0,1,9,16,100]
示例 2： 输入：nums = [-7,-3,2,3,11] 输出：[4,9,9,49,121]

## 思路
数组其实是有序的， 只不过负数平方之后可能成为最大数了。那么**数组平方的最大值就在数组的两端，不是最左边就是最右边，不可能是中间**。
所以可以考虑**双指针法**了，i指向起始位置，j指向终止位置。
定义一个新数组result，和A数组一样的大小，让k指向result数组终止位置。

- 如果A[i] * A[i] < A[j] * A[j] 那么result[k--] = A[j] * A[j]; 。
- 如果A[i] * A[i] >= A[j] * A[j] 那么result[k--] = A[i] * A[i]; 。

如动画所示：
![](leetcode997.assets/1644988580511-00b3ff20-48d4-42cf-a370-c9fa5f39d282.gif)
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        int retIndex = A.size() - 1;
        int left = 0;
        int right = retIndex;
        vector<int> ret(A.size(), 0);
        while (left <= right)
        {
            if (A[left]*A[left] <= A[right]*A[right])
            {
                ret[retIndex--] = A[right]*A[right];
                right--;
            }
            else
            {
                ret[retIndex--] = A[left]*A[left];
                left++;
            }
        }
        return ret;
    }
};
```
