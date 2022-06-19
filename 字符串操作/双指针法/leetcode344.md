## 题目
[344 反转字符串](https://leetcode-cn.com/problems/reverse-string/submissions/)
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。
不要给另外的数组分配额外的空间，你必须**原地修改输入数组**、使用 O(1)的额外空间解决这一问题。

示例 1：
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
## 思路
一看到原地修改，优先考虑**双指针法**。其实本题与数组里的双指针题目一样，仅仅是数组换成了vector，数字换成了char，本质不变。
设置两个指针，一个left从左到右，一个right从右到左，依次交换元素值即可：
```cpp
class Solution
{
public:
    void reverseString(vector<char> &s)
    {
        int size = s.size();
        char tmp;
        for (int left = 0, right = size - 1; left < right;left++, right--)
        {
            tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
};
```
