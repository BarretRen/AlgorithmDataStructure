## 题目
[501 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/submissions/)
给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。
**如果树中有不止一个众数，可以按 任意顺序 返回**。
## 思路
二叉搜索树中序遍历是有序数组，所以可以在遍历过程中进行出现最大次数的判断（相当于判断有序数组出现的次数）。遍历有序数组的元素出现频率，从头遍历，那么一定是相邻两个元素作比较，然后就把出现频率最高的元素输出就可以了。
在leetcode 530已经使用了pre指针和cur指针的技巧，这次又用上了。
弄一个指针指向前一个节点，这样每次cur（当前节点）才能和pre（前一个节点）作比较。而且初始化的时候pre = NULL，这样当pre为NULL时候，我们就知道这是比较的第一个元素。
```cpp
class Solution
{
public:
    vector<int> findMode(TreeNode *root)
    {
        count = 0;
        maxCount = 0;
        pre = nullptr;
        vec.clear();

        inOrderSearchTree(root);
        return vec;
    }

private:
    int count, maxCount;
    vector<int> vec;
    TreeNode *pre;
    void inOrderSearchTree(TreeNode *node)
    {
        if (node == nullptr)
            return;

        //中序遍历
        inOrderSearchTree(node->left);
        //处理当前node
        if (pre == nullptr)
            count = 1;// 第一个节点
        else if (pre->val == node->val)
            count++;// 与前一个节点数值相同，计数加1
        else
            count = 1;//与前一个节点不同,重置为1

        pre = node;// 更新上一个节点

        if (count == maxCount)
        {
            vec.push_back(node->val);// 如果和最大值相同,说明又找到一个众数,放进result中
        }
        else if (count > maxCount)// 如果计数大于最大值,说明当前vec存的不是众数
        {
            maxCount = count;
            vec.clear();
            vec.push_back(node->val);
        }
        inOrderSearchTree(node->right);
    }
};
```

