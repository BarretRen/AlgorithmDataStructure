## 题目
[1047 删除字符串中的所有相邻重复项](https://leetcode-cn.com/problems/remove-all-adjacent-duplicates-in-string/submissions/)
给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。
在 S 上反复执行重复项删除操作，直到无法继续删除。在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。
示例：

- 输入："abbaca"
- 输出："ca"
## 思路
本题要删除相邻相同元素，其实也是匹配问题，相同左元素相当于左括号，相同右元素就是相当于右括号，匹配上了就删除。
本题可以把字符串顺序放到一个栈中，然后如果相同的话 栈就弹出，这样最后栈里剩下的元素都是相邻不相同的元素了。
![](leetcode1047.assets/1645596649598-e95feab2-aba3-4817-a1e6-22752a9b81a4.gif)
所以本题用栈解决
### 用stack解决
```cpp
class Solution
{
public:
    string removeDuplicates(string s)
    {
        std::stack<int> st;
        for (auto c : s)
        {
            if (!st.empty() && st.top() == c)
            {
                st.pop();
            }
            else
            {
                st.push(c);
            }
        }
        string result = "";
        while (!st.empty())
        { // 将栈中元素放到result字符串
            result += st.top();
            st.pop();
        }
        reverse(result.begin(), result.end()); // 此时字符串需要反转一下
        return result;
    }
};
```
### 用string解决
**想不到吧，C++11之后的string可以直接字符串末尾进行增删，也可以模拟stack的一些类似特性。**本题用string代替栈，可以省去了栈还要转为字符串和反转字符串的操作
```cpp
class Solution
{
public:
    string removeDuplicates(string S)
    {
        string result;
        for (char s : S)
        {
            if (result.empty() || result.back() != s)
            {
                result.push_back(s);
            }
            else
            {
                result.pop_back();
            }
        }
        return result;
    }
};
```
