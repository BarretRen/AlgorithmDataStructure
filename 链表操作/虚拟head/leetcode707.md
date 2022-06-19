## 题目
[707 设计链表](https://leetcode-cn.com/problems/design-linked-list/)
在链表类中实现这些功能：

- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点
## 思路
由题目能看出，index从0开始，范围是`[0, size - 1]`。
为了使代码更整洁，使用**虚拟头节点法**，主要需要注意index的范围，不要在nullptr位置做增删操作。单链表的实现如下
```cpp
class MyLinkedList
{
public:
    MyLinkedList()
    {
        size = 0;
        head = new ListNode(0); //虚拟头节点，不保存val
    }
    // 获取到第index个节点数值，如果index是非法数值直接返回-1
    // index是从0开始的，第0个节点就是头结点
    int get(int index)
    {
        ListNode *tmp = getNode(index);
        if (tmp != nullptr)
            return tmp->val;
        else
            return -1;
    }

    void addAtHead(int val)
    {
        ListNode *node = new ListNode(val, head->next);
        head->next = node;
        size++;
    }

    void addAtTail(int val)
    {
        ListNode *node = new ListNode(val, head->next);
        ListNode *tmp = getNode(size - 1);
        if (tmp == nullptr)
            tmp = head;
        tmp->next = node;
        size++;
    }
    // 在第index个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果index大于链表的长度，则返回空
    void addAtIndex(int index, int val) // index从0开始
    {
        if (index > size)
        {
            return;
        }

        ListNode *node = new ListNode(val);
        ListNode *tmp = getNode(index - 1);
        if (tmp == nullptr)
            tmp = head;
        node->next = tmp->next;
        tmp->next = node;
        size++;
    }
    // 删除第index个节点，如果index 大于等于链表的长度，直接return
    // 注意index是从0开始的
    void deleteAtIndex(int index) // index从0开始
    {
        if (index >= size || index < 0)
        {
            return;
        }
        ListNode *tmp = getNode(index - 1);
        if (tmp == nullptr)
            tmp = head;
        ListNode *node = tmp->next;
        tmp->next = node->next;
        delete node;
        size--;
    }

private:
    struct ListNode
    {
        int val;
        ListNode *next;
        ListNode(int val) : val(val), next(nullptr) {}
        ListNode(int val, ListNode *n) : val(val), next(n) {}
    };
    int size;
    ListNode *head;

    ListNode *getNode(int index)
    {
        if (index >= size || index < 0)
        {
            return nullptr;
        }
        ListNode *tmp = head;
        while (index >= 0)
        {
            tmp = tmp->next;
            index--;
        }
        return tmp;
    }
};
```
