# Leetcode 295 周赛

#### [2287. 重排字符形成目标字符串](https://leetcode.cn/problems/rearrange-characters-to-make-target-string/)

难度简单8

给你两个下标从 **0** 开始的字符串 `s` 和 `target` 。你可以从 `s` 取出一些字符并将其重排，得到若干新的字符串。

从 `s` 中取出字符并重新排列，返回可以形成 `target` 的 **最大** 副本数。

 

**示例 1：**

```
输入：s = "ilovecodingonleetcode", target = "code"
输出：2
解释：
对于 "code" 的第 1 个副本，选取下标为 4 、5 、6 和 7 的字符。
对于 "code" 的第 2 个副本，选取下标为 17 、18 、19 和 20 的字符。
形成的字符串分别是 "ecod" 和 "code" ，都可以重排为 "code" 。
可以形成最多 2 个 "code" 的副本，所以返回 2 。
```

**示例 2：**

```
输入：s = "abcba", target = "abc"
输出：1
解释：
选取下标为 0 、1 和 2 的字符，可以形成 "abc" 的 1 个副本。 
可以形成最多 1 个 "abc" 的副本，所以返回 1 。
注意，尽管下标 3 和 4 分别有额外的 'a' 和 'b' ，但不能重用下标 2 处的 'c' ，所以无法形成 "abc" 的第 2 个副本。
```

**示例 3：**

```
输入：s = "abbaccaddaeea", target = "aaaaa"
输出：1
解释：
选取下标为 0 、3 、6 、9 和 12 的字符，可以形成 "aaaaa" 的 1 个副本。
可以形成最多 1 个 "aaaaa" 的副本，所以返回 1 。
```

 

**提示：**

- `1 <= s.length <= 100`
- `1 <= target.length <= 10`
- `s` 和 `target` 由小写英文字母组成



## 题解

木桶原理 s 中 最少频率的 target 元素 就是能形成的字符串个数



~~~python
from collections import defaultdict
class Solution:
    def rearrangeCharacters(self, s: str, target: str) -> int:
        total = 0
        result = defaultdict(int)
        ## 找到所有需要的字母 和 有的 个数
        for i in s:
            if i in target:
                result[i] += 1
                
        flag = 1
        while flag:
            for i in target:
                if i not in target:
                    return 0
                if result[i] == 0:
                    flag = 0
                    return total
                else:
                    result[i] -= 1
            total += 1
        return total
~~~





#### [2288. 价格减免](https://leetcode.cn/problems/apply-discount-to-prices/)

难度中等6

**句子** 是由若干个单词组成的字符串，单词之间用单个空格分隔，其中每个单词可以包含数字、小写字母、和美元符号 `'$'` 。如果单词的形式为美元符号后跟着一个非负实数，那么这个单词就表示一个价格。

- 例如 `"$100"`、`"$23"` 和 `"$6.75"` 表示价格，而 `"100"`、`"$"` 和 `"2$3"` 不是。

**注意：**本题输入中的价格均为整数。

给你一个字符串 `sentence` 和一个整数 `discount` 。对于每个表示价格的单词，都在价格的基础上减免 `discount%` ，并 **更新** 该单词到句子中。所有更新后的价格应该表示为一个 **恰好保留小数点后两位** 的数字。

返回表示修改后句子的字符串。

 

**示例 1：**

```
输入：sentence = "there are $1 $2 and 5$ candies in the shop", discount = 50
输出："there are $0.50 $1.00 and 5$ candies in the shop"
解释：
表示价格的单词是 "$1" 和 "$2" 。 
- "$1" 减免 50% 为 "$0.50" ，所以 "$1" 替换为 "$0.50" 。
- "$2" 减免 50% 为 "$1" ，所以 "$1" 替换为 "$1.00" 。
```

**示例 2：**

```
输入：sentence = "1 2 $3 4 $5 $6 7 8$ $9 $10$", discount = 100
输出："1 2 $0.00 4 $0.00 $0.00 7 8$ $0.00 $10$"
解释：
任何价格减免 100% 都会得到 0 。
表示价格的单词分别是 "$3"、"$5"、"$6" 和 "$9"。
每个单词都替换为 "$0.00"。
```

 

**提示：**

- `1 <= sentence.length <= 105`
- `sentence` 由小写英文字母、数字、`' '` 和 `'$'` 组成
- `sentence` 不含前导和尾随空格
- `sentence` 的所有单词都用单个空格分隔
- 所有价格都是 **正** 整数且不含前导零
- 所有价格 **最多** 为 `10` 位数字
- `0 <= discount <= 100`



## 题解

主要就是 模拟 . 先把 str 以空格进行分割

然后对列表中每一个元素进行判断 

遇到需要替换元素时候 要注意 最后结果需要保留两位小数



~~~python
class Solution:
    def discountPrices(self, sentence: str, discount: int) -> str:
        DEBUG = 0
        new = sentence.split(" ")
        if DEBUG:
            print(new)
        final = ""
        for i in new:
            if i[0] == '$' and i[1:].isdigit():
                if DEBUG:
                    print(i)
                final += '$'
                final += str(format(float(i[1:]) * (1 - discount * 0.01), '.2f'))
            else:
                final += i
            final += " "
        return final[:-1]
~~~





#### [2289. 使数组按非递减顺序排列](https://leetcode.cn/problems/steps-to-make-array-non-decreasing/)

难度中等65

给你一个下标从 **0** 开始的整数数组 `nums` 。在一步操作中，移除所有满足 `nums[i - 1] > nums[i]` 的 `nums[i]` ，其中 `0 < i < nums.length` 。

重复执行步骤，直到 `nums` 变为 **非递减** 数组，返回所需执行的操作数。

 

**示例 1：**

```
输入：nums = [5,3,4,4,7,3,6,11,8,5,11]
输出：3
解释：执行下述几个步骤：
- 步骤 1 ：[5,3,4,4,7,3,6,11,8,5,11] 变为 [5,4,4,7,6,11,11]
- 步骤 2 ：[5,4,4,7,6,11,11] 变为 [5,4,7,11,11]
- 步骤 3 ：[5,4,7,11,11] 变为 [5,7,11,11]
[5,7,11,11] 是一个非递减数组，因此，返回 3 。
```

**示例 2：**

```
输入：nums = [4,5,7,7,13]
输出：0
解释：nums 已经是一个非递减数组，因此，返回 0 。
```

 

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`



## 题解

个人认为这题蛮难的, 看了别人的题解花了好久也不是很能理解, 可能因为之前没有了解过***单调栈***和***线段树***.

这里说下我自己的理解

*例子*: [20, 1, 9, 1, 2, 3]

我们需要的是单调非递减排列

所以 在什么什么时候会删除元素呢?

如果 由以下情况出现 x, y, x 出现在 y 之前, 且 x > y. 那么 y 就需要被删除.

所以我们这里额外使用一个栈, 栈首是 会导致删除其他后面元素的 元素, 和删除他的时间

*例子*: [20, 1, 9, 1, 2, 3]

首先 迭代是 一个 是 20, 他是 首元素 可能导致删除后面的元素, 所以 放入栈中, 且它的删除时间 应该 为 0, 因为他不会被删除

那么现在栈应该就是 [[20, 0]]

接下来是 1, 我们 发现 1 < 20, 所以他会被 20 删除, 我们要做下记录, 并放入栈中

那么现在栈里就应该是 [[20, 0], [1, 1]]

接下来是 9, 我们可以发现 9 大于 1, 所以 他不会被 1 删除, 但是他会被 20 删除, 且它会在 1 被删除之后删除

那么现在栈里就应该是 [[20, 0], [9, 2]]

接下来 是 1, 他会因为 9 被删除, 但要注意的是 他应该是在第一次被删除, 因为 9 和 1 之间没有其他元素, 是 9 直接导致 他被删除,

那么现在栈里就应该是 [[20, 0], [9, 2], [1, 1]]

接下来是 2我们可以发现 2 大于 1, 但是小于 9, 所以他应该是在 1 被删除之后删除, 所以他应该是在第二次被删除

那么现在栈里就应该是 [[20, 0], [9, 2], [2, 2]]

最后是3, 同理 3 大于 2, 他应该在 2 之后删除, 所以 他应该是在 第三次被删除



综上, 我的理解就是 ***放入栈中 都是去找到底是谁删除我的***





~~~python
class Solution:
    def totalSteps(self, nums: List[int]) -> int:
        DEBUG = 0
        ans = 0
        ## stack 中储存两个 值 与第一个 是值, 第二个是 当前这个值是在什么时候被删除的
        stack = []

        for i in nums:
            current = 0
            ## 如果 栈里有值, 而且当前这个值还大于了 栈内最后一个值, 就要把之前一个值删掉
            ## 我的理解是 在这里我们需要让 stack 单调递减
            while stack and stack[-1][0] <= i:
                current = max(current, stack.pop()[1])
            
            ## 如果这时候stack为空 说明当前的值不会被之前的值删掉
            current = current + 1 if stack else 0

            ## 更新结果
            ans = max(ans, current)

            stack.append([i, current])

        return ans
~~~

 



#### [2290. 到达角落需要移除障碍物的最小数目](https://leetcode.cn/problems/minimum-obstacle-removal-to-reach-corner/)

难度困难25

给你一个下标从 **0** 开始的二维整数数组 `grid` ，数组大小为 `m x n` 。每个单元格都是两个值之一：

- `0` 表示一个 **空** 单元格，
- `1` 表示一个可以移除的 **障碍物** 。

你可以向上、下、左、右移动，从一个空单元格移动到另一个空单元格。

现在你需要从左上角 `(0, 0)` 移动到右下角 `(m - 1, n - 1)` ，返回需要移除的障碍物的 **最小** 数目。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2022/04/06/example1drawio-1.png)

```
输入：grid = [[0,1,1],[1,1,0],[1,1,0]]
输出：2
解释：可以移除位于 (0, 1) 和 (0, 2) 的障碍物来创建从 (0, 0) 到 (2, 2) 的路径。
可以证明我们至少需要移除两个障碍物，所以返回 2 。
注意，可能存在其他方式来移除 2 个障碍物，创建出可行的路径。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/04/06/example1drawio.png)

```
输入：grid = [[0,1,0,0,0],[0,1,0,1,0],[0,0,0,1,0]]
输出：0
解释：不移除任何障碍物就能从 (0, 0) 到 (2, 4) ，所以返回 0 。
```

 

**提示：**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 105`
- `2 <= m * n <= 105`
- `grid[i][j]` 为 `0` **或** `1`
- `grid[0][0] == grid[m - 1][n - 1] == 0`







## 题解

这题 我一开始是想用回溯, 但是超时, 而且第二次想使用回溯的时候发现写不出来了

所以代码 还是要好好练习啊

然后在别人那看到一个很好的思路 是用 BFS

我们可以吧有障碍物的格子设定为权重为 1 的路径, 没有障碍物的格子是权重为0.

所以我们可以把问题转换为 从起点到终点的最短路径

通过 BFS 我们一直到一个最短路径树, 他的每层节点到起点的位置应该相同

这里我们可以使用 deque 来完成这个操作

因为 deque 可以从两端入队, 如果当前路径的权重为0 我们应该优先访问 所以需要从 左入队

而相反 如果权重为 1 , 我们应该之后访问 所以从右入队 以下是推导最短路径树的过程

![2290 最短路径树的推导过程](/Users/andy/Desktop/leetcode/2290 最短路径树的推导过程.jpg)



~~~ python
from collections import deque
class Solution:
    def minimumObstacles(self, grid: List[List[int]]) -> int:

        DEBUG = 0
        m = len(grid)
        n = len(grid[0])

        ## 去储存 到每个点的最短路径
        dp = [[float('inf') for i in range(n)] for i in range(m)]
        dp[0][0] = 0

        ## 上下左右四个方向
        directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]

        ## 用来做 BFS
        queue = deque([[0, 0]])
        while queue:
            curr = queue.popleft()
            for x, y in directions:
                if DEBUG:
                    print(x, y)
                    print(curr)
                new_x = x + curr[0]
                new_y = y + curr[1]

                if 0 <= new_x < m and 0 <= new_y < n:
                    g = grid [new_x][new_y]
                    if g + dp[curr[0]][curr[1]] < dp[new_x][new_y]:
                        dp[new_x][new_y] = g + dp[curr[0]][curr[1]]
                        if dp[new_x][new_y]:
                            if DEBUG:
                                print(new_x, new_y)
                            queue.append([new_x, new_y])
                        else:
                            queue.appendleft([new_x, new_y])
        return dp[m - 1][n - 1]
~~~





思路二 (代码还未实现)

因为可以转换成图论问题, 所以可以使用 Dijkstra 算法实现

这里还没有用代码实现

