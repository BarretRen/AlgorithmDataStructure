## 题目
[151 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)
给定一个字符串，逐个翻转字符串中的每个单词。
示例 1：
输入: "the sky is blue"
输出: "blue is sky the"
示例 2：
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
## 思路
这道题目可以说是综合考察了字符串的多种操作。
不使用辅助空间之后，我们可以将整个字符串都反转过来，那么单词的顺序指定是倒序了，只不过单词本身也倒序了，那么再把单词反转一下，单词不就正过来了。
所以解题变成了如下几个步骤：

- 移除多余空格
- 将整个字符串反转
- 将每个单词反转
### 双指针移除多余空格
使用双指针法，left代表下一个要保存的字符的位置，right用于遍历所有字符。这样移除空格就像leetcode 26里移除数组中重复项一样，注意left的移动即可。
```cpp
//去掉首尾和中间多余空格, 不能使用string.erase, 在循环里用了之后index就失效了
//使用双指针法，left指针用于保存去除多余空格之后的字符
int left = 0, right = 0;
while (right < s.size())
{
    if (s[right] == ' ') //空格继续遍历,不保存,left不变
    {
        right++;
        continue;
    }
    else if (left == 0)
    {
        s[left++] = s[right++];//开头的空格一个不留，直接index0保存字母
    }
    else 
    {
        if (s[right - 1] == ' ') //如果前一个为空格,说明之前遍历的是多个空格,需要保留一个空格
            s[left++] = ' ';
        s[left++] = s[right++]; //保存其他字符,并left向后移动
    }
}
s.resize(left); //重新设置大小，去掉末尾的空格和多余的字母
```
### 反转整个字符串
反转整个字符串比较简单，可以用leetcode344中的双指针方式：
```cpp
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
//整体反转字符串
reverseRangeString(s, 0, s.size() - 1);
```
### 反转每个单词
和上面一样，只需要确认每个单词的两个边界即可
```cpp
//遍历,反转每个单词
for (int i = 0; i < s.size(); i++)
{
    int j = i;
    // 查找单词间的空格，翻转单词
    while (j < s.size() && s[j] != ' ')
        j++;
    reverseRangeString(s, i, j - 1);
    i = j;
}
```
