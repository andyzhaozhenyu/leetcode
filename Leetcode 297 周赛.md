# Leetcode 297 周赛

#### [2303. 计算应缴税款总额](https://leetcode.cn/problems/calculate-amount-paid-in-taxes/)

难度简单2

给你一个下标从 **0** 开始的二维整数数组 `brackets` ，其中 `brackets[i] = [upperi, percenti]` ，表示第 `i` 个税级的上限是 `upperi` ，征收的税率为 `percenti` 。税级按上限 **从低到高排序**（在满足 `0 < i < brackets.length` 的前提下，`upperi-1 < upperi`）。

税款计算方式如下：

- 不超过 `upper0` 的收入按税率 `percent0` 缴纳
- 接着 `upper1 - upper0` 的部分按税率 `percent1` 缴纳
- 然后 `upper2 - upper1` 的部分按税率 `percent2` 缴纳
- 以此类推

给你一个整数 `income` 表示你的总收入。返回你需要缴纳的税款总额。与标准答案误差不超 `10-5` 的结果将被视作正确答案。

 

**示例 1：**

```
输入：brackets = [[3,50],[7,10],[12,25]], income = 10
输出：2.65000
解释：
前 $3 的税率为 50% 。需要支付税款 $3 * 50% = $1.50 。
接下来 $7 - $3 = $4 的税率为 10% 。需要支付税款 $4 * 10% = $0.40 。
最后 $10 - $7 = $3 的税率为 25% 。需要支付税款 $3 * 25% = $0.75 。
需要支付的税款总计 $1.50 + $0.40 + $0.75 = $2.65 。
```

**示例 2：**

```
输入：brackets = [[1,0],[4,25],[5,50]], income = 2
输出：0.25000
解释：
前 $1 的税率为 0% 。需要支付税款 $1 * 0% = $0 。
剩下 $1 的税率为 25% 。需要支付税款 $1 * 25% = $0.25 。
需要支付的税款总计 $0 + $0.25 = $0.25 。
```

**示例 3：**

```
输入：brackets = [[2,50]], income = 0
输出：0.00000
解释：
没有收入，无需纳税，需要支付的税款总计 $0 。
```

 

**提示：**

- `1 <= brackets.length <= 100`
- `1 <= upperi <= 1000`
- `0 <= percenti <= 100`
- `0 <= income <= 1000`
- `upperi` 按递增顺序排列
- `upperi` 中的所有值 **互不相同**
- 最后一个税级的上限大于等于 `income`

## 题解

 主要思路就是模拟



~~~python
class Solution:
    def calculateTax(self, brackets: List[List[int]], income: int) -> float:

        DEBUG = 1
        
        n = len(brackets)
        total = 0
        for i in range(n):
            if i == 0:
                if income <= brackets[i][0]:
                    total += (income * 0.01 * brackets[i][1])
                    break
                else:
                    total += (brackets[i][0] * brackets[i][1] * 0.01)
            else:
                if income <= brackets[i][0]:
                    total += ((income - brackets[i - 1][0]) * 0.01 * brackets[i][1])
                    break
                else:
                    total += (( brackets[i][0] - brackets[i - 1][0]) * 0.01 * brackets[i][1])
                
        if DEBUG:
            print(total)

        return total

~~~



周赛后精简了代码思路



~~~python
class Solution:
    def calculateTax(self, brackets: List[List[int]], income: int) -> float:

        DEBUG = 1
        
        n = len(brackets)
        
        ## 先考虑 index 0
        if income <= brackets[0][0]: return income * (0.01 * brackets[0][1])

        total = brackets[0][0] * (0.01 * brackets[0][1])
        for i in range(1, n):
            if income <= brackets[i][0]:
                total += (income - brackets[i - 1][0]) * (0.01 * brackets[i][1])
                return total
            else:
                total += (brackets[i][0] - brackets[i - 1][0]) * (0.01 * brackets[i][1])
        return total

~~~





#### [2304. 网格中的最小路径代价](https://leetcode.cn/problems/minimum-path-cost-in-a-grid/)

难度中等8

给你一个下标从 **0** 开始的整数矩阵 `grid` ，矩阵大小为 `m x n` ，由从 `0` 到 `m * n - 1` 的不同整数组成。你可以在此矩阵中，从一个单元格移动到 **下一行** 的任何其他单元格。如果你位于单元格 `(x, y)` ，且满足 `x < m - 1` ，你可以移动到 `(x + 1, 0)`, `(x + 1, 1)`, ..., `(x + 1, n - 1)` 中的任何一个单元格。**注意：** 在最后一行中的单元格不能触发移动。

每次可能的移动都需要付出对应的代价，代价用一个下标从 **0** 开始的二维数组 `moveCost` 表示，该数组大小为 `(m * n) x n` ，其中 `moveCost[i][j]` 是从值为 `i` 的单元格移动到下一行第 `j` 列单元格的代价。从 `grid` 最后一行的单元格移动的代价可以忽略。

`grid` 一条路径的代价是：所有路径经过的单元格的 **值之和** 加上 所有移动的 **代价之和** 。从 **第一行** 任意单元格出发，返回到达 **最后一行** 任意单元格的最小路径代价*。*

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/04/28/griddrawio-2.png)

```
输入：grid = [[5,3],[4,0],[2,1]], moveCost = [[9,8],[1,5],[10,12],[18,6],[2,4],[14,3]]
输出：17
解释：最小代价的路径是 5 -> 0 -> 1 。
- 路径途经单元格值之和 5 + 0 + 1 = 6 。
- 从 5 移动到 0 的代价为 3 。
- 从 0 移动到 1 的代价为 8 。
路径总代价为 6 + 3 + 8 = 17 。
```

**示例 2：**

```
输入：grid = [[5,1,2],[4,0,3]], moveCost = [[12,10,15],[20,23,8],[21,7,1],[8,1,13],[9,10,25],[5,3,2]]
输出：6
解释：
最小代价的路径是 2 -> 3 。 
- 路径途经单元格值之和 2 + 3 = 5 。 
- 从 2 移动到 3 的代价为 1 。 
路径总代价为 5 + 1 = 6 。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `2 <= m, n <= 50`
- `grid` 由从 `0` 到 `m * n - 1` 的不同整数组成
- `moveCost.length == m * n`
- `moveCost[i].length == n`
- `1 <= moveCost[i][j] <= 100`



## 题解

思路是动态规划, 但感觉代码还是太繁琐, 有时间可以想象更好简化代码, 提高效率



~~~python
from collections import defaultdict
class Solution:
    def minPathCost(self, grid: List[List[int]], moveCost: List[List[int]]) -> int:
        DEBUG = 0

        m = len(grid)
        n = len(grid[0])
        
        dp = [[0 for i in range(n)] for i in range(m)]
        for i in range(n):
            dp[0][i] = grid[0][i]
   
        if DEBUG:
            print(dp)    

        dic = defaultdict(list)
        for i, j in enumerate(moveCost):
            dic[i] = j
        
        if DEBUG:
            print(dic)
        
        for i in range(1, m):
            for j in range(n):
                dp[i][j] += grid[i][j]
                ## 这里考虑状态转移, 从上一行到这一行的最小值
                curr = float(inf)
                for k in range(len(grid[i - 1])):
                    curr = min(curr, dp[i - 1][k] + dic[grid[i - 1][k]][j])
                dp[i][j] += curr

        if DEBUG:
            print(dp)
        
        return min(dp[-1])


~~~





#### [2305. 公平分发饼干](https://leetcode.cn/problems/fair-distribution-of-cookies/)

难度中等18

给你一个整数数组 `cookies` ，其中 `cookies[i]` 表示在第 `i` 个零食包中的饼干数量。另给你一个整数 `k` 表示等待分发零食包的孩子数量，**所有** 零食包都需要分发。在同一个零食包中的所有饼干都必须分发给同一个孩子，不能分开。

分发的 **不公平程度** 定义为单个孩子在分发过程中能够获得饼干的最大总数。

返回所有分发的最小不公平程度。

 

**示例 1：**

```
输入：cookies = [8,15,10,20,8], k = 2
输出：31
解释：一种最优方案是 [8,15,8] 和 [10,20] 。
- 第 1 个孩子分到 [8,15,8] ，总计 8 + 15 + 8 = 31 块饼干。
- 第 2 个孩子分到 [10,20] ，总计 10 + 20 = 30 块饼干。
分发的不公平程度为 max(31,30) = 31 。
可以证明不存在不公平程度小于 31 的分发方案。
```

**示例 2：**

```
输入：cookies = [6,1,3,2,2,4,1,2], k = 3
输出：7
解释：一种最优方案是 [6,1]、[3,2,2] 和 [4,1,2] 。
- 第 1 个孩子分到 [6,1] ，总计 6 + 1 = 7 块饼干。 
- 第 2 个孩子分到 [3,2,2] ，总计 3 + 2 + 2 = 7 块饼干。
- 第 3 个孩子分到 [4,1,2] ，总计 4 + 1 + 2 = 7 块饼干。
分发的不公平程度为 max(7,7,7) = 7 。
可以证明不存在不公平程度小于 7 的分发方案。
```

 

**提示：**

- `2 <= cookies.length <= 8`
- `1 <= cookies[i] <= 105`
- `2 <= k <= cookies.length`

