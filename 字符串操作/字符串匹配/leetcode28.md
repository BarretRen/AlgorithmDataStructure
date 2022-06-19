## 题目
[28 实现strstr()](https://leetcode-cn.com/problems/implement-strstr/)
给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
示例 1: 输入: haystack = "hello", needle = "ll" 输出: 2
示例 2: 输入: haystack = "aaaaa", needle = "bba" 输出: -1
## 思路
字符串匹配的问题，想要高效率就考虑KMP算法。在KMP算法中主要是生成next数组，根据next数组形式的不同，本题有以下两种KMP的实现方式
### 原始前缀表
```cpp
class Solution
{
public:
    int strStr(string s, string t)
    {
        if (t.size() == 0)
        {
            return 0;
        }
        int next[t.size()];
        getNext(next, t);

        int j = 0; // next为原始前缀表,所以j从0开始
        for (int i = 0; i < s.size(); i++)
        {
            while (j > 0 && s[i] != t[j])
            {
                j = next[j - 1];
            }
            if (s[i] == t[j])
            {
                // 匹配，j和i同时向后移动
                j++; // i++在for循环里
            }
            if (j == t.size())
            {
                // j指向了模式串t的末尾，那么就说明模式串t完全匹配文本串s里的某个子串
                return (i - t.size() + 1); //返回匹配子串的头index
            }
        }
        return -1;
    }

private:
    //获取原始前缀表
    void getNext(int *next, const string &s)
    {
        int j = 0; // j指向前缀起始位置
        next[0] = 0;
        // i指向后缀起始位置,i从1开始
        for (int i = 1; i < s.size(); i++)
        {
            while (j > 0 && s[i] != s[j])
            {
                //前后缀不相同的情况
                j = next[j - 1]; // 向前回退,找j+1前一个元素在next数组里的值
            }
            if (s[i] == s[j])
            {
                //前后缀相同的情况
                j++; // i和j向后移动,i++在for里面
            }
            next[i] = j;
        }
    }
};
```
### 前缀有统一减一
```cpp
class Solution
{
public:
    int strStr(string s, string t)
    {
        if (t.size() == 0)
        {
            return 0;
        }
        int next[t.size()];
        getNext(next, t);

        //在文本串s里找是否出现过模式串t
        int j = -1; // 因为next数组里记录的起始位置为-1
        // i从0开始遍历文本串s
        for (int i = 0; i < s.size(); i++)
        {
            while (j >= 0 && s[i] != t[j + 1])
            {
                // 不匹配
                j = next[j]; // j 寻找之前匹配的位置
            }
            if (s[i] == t[j + 1])
            {
                // 匹配，j和i同时向后移动
                j++; // i++在for循环里
            }
            if (j == (t.size() - 1))
            {
                // j指向了模式串t的末尾，那么就说明模式串t完全匹配文本串s里的某个子串
                return (i - t.size() + 1); //返回匹配子串的头index
            }
        }
        return -1;
    }

private:
    //获取前缀表统一减一形式的next数组
    void getNext(int *next, const string &s)
    {
        int j = -1; // j指向前缀起始位置
        next[0] = j;
        // i指向后缀起始位置,i从1开始
        for (int i = 1; i < s.size(); i++)
        {
            while (j >= 0 && s[i] != s[j + 1])
            {
                //前后缀不相同的情况
                j = next[j]; // 向前回退,找j+1前一个元素在next数组里的值
            }
            if (s[i] == s[j + 1])
            {
                //前后缀相同的情况
                j++; // i和j向后移动,i++在for里面
            }
            next[i] = j; // 将j（前缀的长度）赋给next[i]
        }
    }
};
```
