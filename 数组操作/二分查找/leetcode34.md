## 题目
[34 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)<br />给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。<br />如果数组中不存在目标值 target，返回 [-1, -1]。<br />进阶：你可以设计并实现时间复杂度为`O(\log n)`的算法解决此问题吗？

示例 1：<br />输入：nums = [5,7,7,8,8,10], target = 8<br />输出：[3,4]

## 思路
寻找target在数组里的左右边界，有如下三种情况：

- target 在 nums[0] ~ nums[n-1] 中，nums 中存在 target。例如 nums = [5,7,7,8,8,10]，target = 8，返回 [3，4]。
- target 在 nums[0] ~ nums[n-1] 中，nums 中不存在 target。例如 nums = [5,7,7,8,8,10]，target = 6，返回 [-1,-1]。
- target < nums[0] 或者 target > nums[n-1]。例如 nums = [5,7,7,8,8,10], target = 4，返回 [-1,-1]。

专注于寻找右区间，然后专注于寻找左区间，左右根据左右区间做最后判断。不要上来就想如果一起寻找左右区间，搞着搞着就会顾此失彼，绕进去拔不出来了.
```cpp
//二分查找左右边界
/*
1.target比数组里所有元素都小;
2.target比数组里所有元素都大；
3.target在数组元素之间，但是没有值和target相等的元素；
4.能够找到和target相等的元素，且唯一；
5.能够找到和target相等的元素，但不唯一；
*/
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int leftborder = LeftBorder(nums, target);
        int rightborder = RightBorder(nums, target);
        //情况1和情况2和情况3
        if(leftborder==-2 || rightborder==-2) return {-1,-1};
        //情况4和5
        else return {leftborder,rightborder};
    }
private:
    //寻找左边界（包括target元素本身）
    int LeftBorder(vector<int>& nums, int target){
        int left = 0; //上限
        int right = nums.size()-1; //下限
        int leftborder = -2; //初始化左边界值为-2
        while(left<=right){
            int mid = left+((right-left)/2); //中间
            //等于的时候也缩小下限，往目标元素值左边逼近
            if(nums[mid]==target){
                leftborder = mid; //不断更新边界值
                right = mid-1; //接着缩小范围，看看它左边还有没有等于target的元素
            }
            else if(nums[mid]>target){
                right = mid-1;
            }
            else left = mid+1;
        }
        //最后返回左边界的索引
        return leftborder;
    }
    //寻找右边界（包括target元素本身）
    int RightBorder(vector<int>& nums, int target){
        int left = 0; //上限
        int right = nums.size()-1; //下限
        int rightborder = -2; //初始化右边界值为-2
        while(left<=right){
            int mid = left+((right-left)/2); //中间
            //等于的时候也缩小下限，往右边逼近
            if(nums[mid]==target){
                rightborder = mid; //不断更新边界值
                left = mid+1; //接着缩小范围，看看它右边还有没有等于target的元素
            }
            else if(nums[mid]<=target){
                left = mid+1;
            }
            else right = mid-1;
        }
        //最后返回右边界的索引
        return rightborder;
    }
};
```

普通二分查找是，当 nums[mid] == target 时，直接返回 mid，而在本题中，则是要继续向左或向右查找，看是否还有和target相等的数组元素。
