## 题目
[242有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/solution/you-xiao-de-zi-mu-yi-wei-ci-by-leetcode-solution/)
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

示例 1:
输入: s = "anagram", t = "nagaram" 输出: true
## 思路
### 用数组
定义一个数组叫做record用来上记录字符串s里字符出现的次数。

- 需要把字符映射到数组也就是哈希表的索引下标上，**因为字符a到字符z的ASCII是26个连续的数值，所以字符a映射为下标0，相应的字符z映射为下标25。**
- 再遍历字符串s时，**只需要将 s[i] - ‘a’ 所在的元素做+1 操作即可，并不需要记住字符a的ASCII，只要求出一个相对数值就可以了。** 这样就将字符串s中字符出现的次数，统计出来了。
- 同样在遍历字符串t的时候，对t中出现的字符映射哈希表索引上的数值再做-1的操作。
- 最后检查一下，**record数组**
   - **如果有的元素不为零0，说明字符串s和t一定是谁多了字符或者谁少了字符，return false。**
   - 如果record数组所有元素都为零0，说明字符串s和t是字母异位词，return true
```cpp
class Solution
{
public:
    bool isAnagram(string s, string t)
    {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++)
        {
            // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++)
        {
            record[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++)
        {
            if (record[i] != 0)
            {
                // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        // record数组所有元素都为零0，说明字符串s和t是字母异位词
        return true;
    }
};
```
### hashmap
依然是上面的思路，但是我们可以直接用`unordered_map`来记录哪些字符出现和出现的次数，原理和使用数组一样，不过这里是hashmap的实现。
虽然在这道题目看来两种解法没有区别，但是如果哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。
```cpp
class Solution
{
public:
    bool isAnagram(string s, string t)
    {
        if (s.size() != t.size())
            return false;

        std::unordered_map<char, int> nums;
        for (auto it = s.begin(); it != s.end(); it++)
        {
            if (nums.find(*it) == nums.end())
                nums[*it] = 1;
            else
                nums[*it]++;
        }

        for (auto it = t.begin(); it != t.end(); it++)
        {
            if (nums.find(*it) == nums.end())
                return false;
            else if (nums[*it] == 0)
                return false;
            else
                nums[*it]--;
        }
        return true;
    }
};
```
