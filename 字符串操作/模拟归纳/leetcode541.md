## 题目
[541 反转字符串II](https://leetcode-cn.com/problems/reverse-string-ii/)
给定一个字符串 s 和一个整数 k，从字符串开头算起，每计数至 2k 个字符，就反转这 2k 字符中的前 k 个字符。
如果剩余字符少于 k 个，则将剩余字符全部反转。
如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样。
 

示例 1：
输入：s = "abcdefg", k = 2
输出："bacdfeg"
## 思路
这道题目其实也是模拟，实现题目中规定的反转规则就可以了。
一些同学可能为了处理逻辑：每隔2k个字符的前k的字符，写了一堆逻辑代码或者再搞一个计数器，来统计2k，再统计前k个字符(我一开始就是这么写, 唉)。
**其实在遍历字符串的过程中，只要让 i += (2 * k)，i 每次移动 2 * k 就可以了，然后判断是否需要有反转的区间**。模拟分析实际的流程后, 判断规则其实就两条:

- 剩余字符多于或等于k个,  则从当前位置开始反转k个
- 剩余字符少于k个, 则全部反转

只需要注意要找的是每2 * k 区间的起点作为当前位置，这样写，程序会高效很多。
```cpp
class Solution
{
public:
    string reverseStr(string s, int k)
    {
        for (int i = 0; i < s.size(); i += (2 * k))
        {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size())
            {
                reverseRangeString(s, i, i + k - 1);
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
            reverseRangeString(s, i, s.size() - 1);
        }
        return s;
    }

private:
    void reverseRangeString(string &s, int l, int r)
    {
        char tmp;
        for (int left = l, right = r; left < right; left++, right--)
        {
            tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
};
```
