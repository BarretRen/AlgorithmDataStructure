## 题目
[206 翻转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表
示例: 输入: 1->2->3->4->5->NULL 

输出: 5->4->3->2->1->NULL

## 思路
并没有添加或者删除节点，仅仅是改变next指针的方向。那么接下来看一看是如何反转的呢？
和数组一样，原地变化最好的解决方法是**双指针法**。拿示例中的链表来举例，如动画所示![](leetcode206.assets/1645172184957-9a7296d0-5975-47d0-b1d2-00ecfacd71f0.gif)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* temp; // 保存cur的下一个节点
        ListNode* cur = head;
        ListNode* pre = NULL;
        while(cur) {
            temp = cur->next;  // 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur->next = pre; // 翻转操作
            // 更新pre 和 cur指针
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```
