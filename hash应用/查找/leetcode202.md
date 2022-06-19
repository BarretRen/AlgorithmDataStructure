## 题目
[202 快乐数](https://leetcode-cn.com/problems/happy-number/submissions/)
编写一个算法来判断一个数 n 是不是快乐数。
快乐数定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

**示例：**
输入：19
输出：true
解释：
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
## 思路
题目中说了会**无限循环**，那么也就是说**求和的过程中，sum会重复出现，这对解题很重要。**
**当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法了。**所以这道题目使用哈希法，来判断这个sum是否重复出现，如果重复了就是return false， 否则一直找到sum为1为止。
```cpp
class Solution
{
public:
    bool isHappy(int n)
    {
        int sum = n;
        unordered_set<int> storedSum;
        while (sum != 1)
        {
            //计算每位的平方和
            int num = sum;
            sum = 0;
            while (num != 0)
            {
                sum += (num % 10) * (num % 10);
                num = num / 10;
            }
            //判断平方和是否已经出现过
            if (storedSum.find(sum) != storedSum.end())
                return false;
            else
                storedSum.insert(sum);
        }
        return true;
    }
};
```
