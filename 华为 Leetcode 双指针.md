# 华为机考题 双指针



#### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

难度中等3537

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

 

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

 

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`



## 题解

思路: 主要使用双指针

一开始让宽带最大, 所以 left = 0, right = Len() - 1

因为宽度在缩小, 所以我们要找到高度最大

~~~pytjon
class Solution:
    def maxArea(self, height: List[int]) -> int:
        DEBUG = 0
        n = len(height)
        left = 0
        right = n - 1

        current_height = float('inf')
        current_max = float('-inf')
        while left < right:
            current_height = min(height[left], height[right])
            current_max = max(current_max, current_height * (right - left))

            if height[left] >= height[right]:
                right -= 1
            else:
                left += 1
        return current_max

~~~







#### [986. 区间列表的交集](https://leetcode.cn/problems/interval-list-intersections/)

难度中等292

给定两个由一些 **闭区间** 组成的列表，`firstList` 和 `secondList` ，其中 `firstList[i] = [starti, endi]` 而 `secondList[j] = [startj, endj]` 。每个区间列表都是成对 **不相交** 的，并且 **已经排序** 。

返回这 **两个区间列表的交集** 。

形式上，**闭区间** `[a, b]`（其中 `a <= b`）表示实数 `x` 的集合，而 `a <= x <= b` 。

两个闭区间的 **交集** 是一组实数，要么为空集，要么为闭区间。例如，`[1, 3]` 和 `[2, 4]` 的交集为 `[2, 3]` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```
输入：firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
输出：[[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
```

**示例 2：**

```
输入：firstList = [[1,3],[5,9]], secondList = []
输出：[]
```

**示例 3：**

```
输入：firstList = [], secondList = [[4,8],[10,12]]
输出：[]
```

**示例 4：**

```
输入：firstList = [[1,7]], secondList = [[3,10]]
输出：[[3,7]]
```

 

**提示：**

- `0 <= firstList.length, secondList.length <= 1000`
- `firstList.length + secondList.length >= 1`
- `0 <= starti < endi <= 109`
- `endi < starti+1`
- `0 <= startj < endj <= 109`
- `endj < startj+1`





## 题解

对两个列表进行指针索引

相交区间开始于 两个区间中较大的 开始值值, 结束于 两个区间中 较早结束的值

然后让 较早结束的区间索引加1

~~~python
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        if not firstList or not secondList:
            return []
        
        result = []

        m = len(firstList)
        n = len(secondList)

        i = 0
        j = 0

        while i < m and j < n:
            start = max(firstList[i][0], secondList[j][0])
            end = min(firstList[i][1], secondList[j][1])
            if start <= end:
                result.append([start, end])
            if firstList[i][1] < secondList[j][1]:
                i += 1
            else:
                j += 1
        return result
~~~





#### [844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

难度简单435

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

 

**示例 1：**

```
输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
```

**示例 2：**

```
输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
```

**示例 3：**

```
输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。
```





## 题解

一开始我想到的时候模拟, 创建两个stack去模拟遇到 ‘#’, 就删除之前元素

~~~python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        DEBUG = 1
        stack1 = []
        for i in s:
            if i == '#':
                if stack1:
                    stack1.pop()
                continue
            else:
                stack1.append(i)
        
        stack2 = []
        for i in t:
            if i == '#':
                if stack2:
                    stack2.pop()
                continue
            else:
                stack2.append(i)
                
        return stack1 == stack2
~~~

但是之后发现这题可以用双指针去解

~~~python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        DEBUG = 1
        i, j = len(s) - 1, len(t) - 1
        skips, skipt = 0, 0
        ## 注意这里不能是 and, 因为只要还有一个 list 有元素没判断完就要继续判断
        
        while i >= 0 or j >= 0:
            while i >= 0:
                ## 如果当前是 回退符
                if s[i] == '#': 
                    skips += 1
                    i -= 1
                ## 这里是要删掉的字符 所以无需比较
                elif skips > 0:
                    skips -= 1
                    i -= 1
                else:
                    break  

            while j >= 0:
                if t[j] == '#':
                    skipt += 1
                    j -= 1
                elif skipt > 0:
                    skipt -= 1
                    j -= 1
                else:
                    break
            if DEBUG:
                print(i, j)
            if i >= 0 and j >= 0:
                if s[i] != t[j]:
                    return False
            ## 这里表明有一个list 已经全部遍历完了
            elif i >= 0 or j >= 0:
                return False

            i -= 1
            j -= 1

        return True
~~~

