## 题目
[257 二叉树的所有路径](https://leetcode-cn.com/problems/binary-tree-paths/submissions/)
给定一个二叉树，返回所有从根节点到叶子节点的路径。
说明: 叶子节点是指没有子节点的节点。

示例
输入：root = [1,2,3,null,5] 
输出：["1->2->5","1->3"]
## 思路
本题用到了回溯的思想。

本题要求从根节点到叶子的路径，所以需要前序遍历，这样才方便让父节点指向孩子节点，找到对应的路径。而且**我们要把路径记录下来，当一个路径在进入另一个路径时需要回溯来回退**。
本题需要如下的递归函数

1. 递归定义:`void getTreePaths(TreeNode* cur, vector<int>& path, vector<string>& result)` 
   - 输入node, 返回记录的路径和结果vector
2. 递归终止条件 
   - node为空,直接返回
   - node左右子节点为空, 根据当前节点和path生成string，添加到result中，返回
3. 不符合递归终止条件，则添加当前节点值到path，继续递归

再看回溯，**回溯和递归是一一对应的，有一个递归，就要有一个回溯**，所以在每次递归返回后，都要进行回溯。
本题中递归只会把当前节点添加到path中，回溯意味着把当前节点从path中删除，不然下次进入递归时path中会存在错误的节点值。
```cpp
class Solution
{
public:
    vector<string> binaryTreePaths(TreeNode *root)
    {
        if (root == nullptr)
            return {};
        vector<int> path; //记录一条路径, 用于回溯
        vector<string> vec;
        getTreePaths(root, path, vec);
        return vec;
    }

private:
    void getTreePaths(TreeNode *cur, vector<int> &path, vector<string> &result)
    {
        if (cur == nullptr) //终止条件1
            return;

        //添加当前节点到path
        path.push_back(cur->val);
        //判断是否符合递归终止条件2
        if (cur->left == nullptr && cur->right == nullptr)
        {
            string tmp;
            for (auto p : path)
            {
                tmp += std::to_string(p) + "->";
            }
            tmp.resize(tmp.size() - 2); //去除末尾->
            result.push_back(tmp);
            return;
        }
        if (cur->left != nullptr)
        {
            //递归左子节点
            getTreePaths(cur->left, path, result);
            //回溯
            path.pop_back(); //这里把左子节点移除
        }
        if (cur->right != nullptr)
        {
            //递归右子节点
            getTreePaths(cur->right, path, result);
            //回溯
            path.pop_back(); //这里把右子节点移除
        }
        return;
    }
};
```
