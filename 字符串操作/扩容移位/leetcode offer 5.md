## 题目
[剑指offer 05 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)
请实现一个函数，把字符串 s 中的每个空格替换成"%20"

示例 1:
输入: s = "We are happy."
输出:"We%20are%20happy."
## 思路
本题本质上是字符替换, 但由于替换前和替换后size不一样, 所以伴随着元素的位移。这类题目有两种解法

- 重新申请new size的内存，与原数组或字符串之间进行错位复制
- **预先给原有数组或字符串扩容为new size大小，然后在从后向前进行位移。**这么做有两个好处：
   1. 不用申请新数组，降低空间复杂度。
   1. 从后向前填充元素，避免了从前先后填充元素要来的每次添加元素都要将添加元素之后的所有元素向后移动。
### 申请新内存法
```cpp
class Solution
{
public:
    string replaceSpace(string s)
    {
        //计算空格的个数
        int count = 0;
        for (int i = 0; i < s.size(); i++)
        {
            if (s[i] == ' ')
                count++;
        }
        //扩充大小
        int newSize = s.size() + count * 2;
        string ret(newSize, '0');
        for (int i = 0, j = 0; i < s.size() && j < newSize;)
        {
            if (s[i] == ' ')
            {
                ret[j++] = '%';
                ret[j++] = '2';
                ret[j++] = '0';
                i++;
            }
            else
            {
                ret[j++] = s[i++];
            }
        }
        return ret;
    }
};
```
### 原数组扩容法
```cpp
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0; // 统计空格的个数
        int sOldSize = s.size();
        for (int i = 0; i < sOldSize; i++) {
            if (s[i] == ' ') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是每个空格替换成"%20"之后的大小
        s.resize(s.size() + count * 2);
        int sNewSize = s.size();
        // 从后先前将空格替换为"%20"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i;) {
            if (s[j] != ' ') {
                s[i--] = s[j--];
            } else {
                s[i--] = '0';
                s[i--] = '2';
                s[i--] = '%';
                j--;
            }
        }
        return s;
    }
};
```
