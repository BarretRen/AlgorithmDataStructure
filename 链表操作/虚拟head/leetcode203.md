## 题目
[203 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/submissions/)
题意：删除链表中等于给定值 val 的所有节点。
示例 1：
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
示例 2：
输入：head = [], val = 1
输出：[]
示例 3：
输入：head = [7,7,7,7], val = 7
输出：[]

## 思路
本题很简单，就是遍历链表查找相等的节点并删除。唯一需要注意的是对head节点的处理。
我们使用**虚拟头节点法**，保证头节点的删除和中间节点的删除保持一致：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution
{
public:
    ListNode *removeElements(ListNode *head, int val)
    {
        ListNode *tmpHead = new ListNode(0, head); //虚拟头节点
        ListNode *index = tmpHead;
        while (index->next != nullptr)
        {
            //当前节点有可能是虚拟头节点，所以直接判断下一节点值，并删除
            if (index->next->val == val)
            {
                ListNode *tmp = index->next;
                index->next = index->next->next;
                delete tmp;
            }
            else
            {
                index = index->next;
            }
        }
        head = tmpHead->next;
        delete tmpHead;
        return head;
    }
};
```
