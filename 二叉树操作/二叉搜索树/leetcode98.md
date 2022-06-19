## 题目
[98 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)
给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

- 节点的左子树只包含 小于 当前节点的数。
- 节点的右子树只包含 大于 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。
## 思路
要知道中序遍历下，输出的二叉搜索树节点的数值是有序序列。有了这个特性，**验证二叉搜索树，就相当于变成了判断一个序列是不是递增的了。**
```cpp
class Solution
{
public:
    bool isValidBST(TreeNode *root)
    {
        vector<int> vec;
        inorder(root, vec);
        int size = vec.size();
        if (size == 1 || size == 0)
            return true;
        for (int i = 1; i < size; i++)
        {
            if (vec[i] <= vec[i - 1])
                return false;
        }
        return true;
    }

private:
    void inorder(TreeNode *root, vector<int> &vec)
    {
        if (root == nullptr)
            return;
        inorder(root->left, vec);
        vec.push_back(root->val);
        inorder(root->right, vec);
    }
};
```
