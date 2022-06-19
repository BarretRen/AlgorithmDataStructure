## 题目
[59 螺旋矩阵II](https://leetcode-cn.com/problems/spiral-matrix-ii/)
给你一个正整数 n ，生成一个包含 1 到`n2` 所有元素，且元素按顺时针顺序螺旋排列的`n x n`正方形矩阵。
示例 1：
![](https://cdn.nlark.com/yuque/0/2022/jpeg/690827/1645021541963-ef220b9b-7e2f-4fa5-88ab-1aba038ab00f.jpeg#clientId=u3d924fe2-004d-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u86aab667&margin=%5Bobject%20Object%5D&originHeight=242&originWidth=242&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u32371679-9a8d-4b89-9a7a-c447dd29215&title=)
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
## 思路
首先分析清楚顺时针画矩阵的过程:

- 填充上行从左到右
- 填充右列从上到下
- 填充下行从右到左
- 填充左列从下到上

由外向内一圈一圈这么画下去。每完成一圈需要画上下左右四条边，并且下一圈时需要将四个边界缩小。

因此我们用代码将这个过程模拟出来（即模拟法）：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/690827/1645024728799-e722184a-e51a-458a-8190-8ffb25133700.png#clientId=uc5613363-6e24-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=357&id=ueacd80e3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=714&originWidth=1070&originalType=url&ratio=1&rotation=0&showTitle=false&size=45369&status=done&style=none&taskId=ubcb690a1-ce02-47c5-947f-9c5b8984b65&title=&width=535)

- 首先确定总循环是从1到`n*n`
- 定义上下左右四个边界，按图中看初始值为`left =0, right = n -1, top =0, bottom = n -1;`
- 当`value <= n*n `时,始终按照 **从左到右 从上到下 从右到左 从下到上（模拟的主过程）**填入顺序循环，每次填入后:
   - value++，得到下一个要填入的值
   - 根据当前填充的是哪一条边界，相应的修改边界的值，缩小边界（top和left需要+1，right和bottom需要减1）
```cpp
class Solution
{
public:
    vector<vector<int>> generateMatrix(int n)
    {
        vector<vector<int>> result(n, vector<int>(n, 0));
        int value = 1;
        int maxValue = n * n;
        int left = 0, right = n - 1, top = 0, bottom = n - 1;
        while (value <= maxValue)
        {
            //从左到右填充上边界
            for (int i = left; i <= right; i++)
                result[top][i] = value++;
            top++;
			//从上到下填充右边界
            for (int i = top; i <= bottom; i++)
                result[i][right] = value++;
            right--;
			//从右到左填充下边界
            for (int i = right; i >= left; i--)
                result[bottom][i] = value++;
            bottom--;
			//从下到上填充左边界
            for (int i = bottom; i >= top; i--)
                result[i][left] = value++;
            left++;
        }
        return result;
    }
};
```
