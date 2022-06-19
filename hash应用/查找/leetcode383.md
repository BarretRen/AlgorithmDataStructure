## 题目
[383 赎金信](https://leetcode-cn.com/problems/ransom-note/)
给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。
如果可以，返回 true ；否则返回 false 。magazine 中的每个字符只能在 ransomNote 中使用一次。

示例 1：
输入：ransomNote = "a", magazine = "b"
输出：false
## 思路
和leetcode242类似，相当于求字符串a和字符串b是否可以相互组成 ，而这道题目是求字符串a能否组成字符串b，而不用管字符串b能不能组成字符串a。
题目中只有小写字母，那可以采用空间换取时间的哈希策略， 用一个长度为26的数组还记录magazine里字母出现的次数。然后再验证这个数组是否包含了ransomNote所需要的所有字母。
依然是数组在哈希法中的应用。
```cpp
class Solution
{
public:
    bool canConstruct(string ransomNote, string magazine)
    {
        int wordNums[26] = {0};
        for (int i = 0; i < magazine.size(); i++)
        {
            wordNums[magazine[i] - 'a']++;
        }

        //验证wordNums中字母能否组成ransomNote
        for (int i = 0; i < ransomNote.size(); i++)
        {
            if (wordNums[ransomNote[i] - 'a'] == 0)
                return false;
            else
                wordNums[ransomNote[i] - 'a']--;
        }
        return true;
    }
};
```
