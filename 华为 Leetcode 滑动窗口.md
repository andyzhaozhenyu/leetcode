# 华为 Leetcode 滑动窗口

#### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

难度中等1189

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其和 `≥ target` 的长度最小的 **连续子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

 

**示例 1：**

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**

```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**

```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

 

**提示：**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

 

**进阶：**

- 如果你已经实现 `O(n)` 时间复杂度的解法, 请尝试设计一个 `O(n log(n))` 时间复杂度的解法。



## 题解

这里运用到了 ***滑动窗口***

首先我们要注意这里要找的是 ***连续子数组***

而且是 ***大于等于***

那我们使用两个指针, 去找这两指针范围内的值是否大于等于 target

如果当前范围 小于 target, 右指针 加 1, 去增大当前范围值

如果 当前范围值 大于等于 target: 减去左指针的索引值, 左指针加 1, 去找是否长度更小的连续子数组和大于等于 target

~~~python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        DEBUG = 0
        
        left, right = 0, 0
        step = float('inf')
        current = 0
        
        while right < len(nums):
            current += nums[right]
            if DEBUG:
                print(current)
            while current >= target:
                step = min(step, right - left + 1)
                current -= nums[left]
                left += 1
            right += 1
        return 0 if step == float('inf') else step
~~~









#### [713. 乘积小于 K 的子数组](https://leetcode.cn/problems/subarray-product-less-than-k/)

难度中等557

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回子数组内所有元素的乘积严格小于 `k` 的连续子数组的数目。

 

**示例 1：**

```
输入：nums = [10,5,2,6], k = 100
输出：8
解释：8 个乘积小于 100 的子数组分别为：[10]、[5]、[2],、[6]、[10,5]、[5,2]、[2,6]、[5,2,6]。
需要注意的是 [10,5,2] 并不是乘积小于 100 的子数组。
```

**示例 2：**

```
输入：nums = [1,2,3], k = 0
输出：0
```

 

**提示:** 

- `1 <= nums.length <= 3 * 104`
- `1 <= nums[i] <= 1000`
- `0 <= k <= 106`





## 题解

这题 和之前类似 还是采用 ***滑动窗口*** 注意到问的是连续子数组

两个指针索引, 如果当前范围指针大于等于 target

那么 消除 left 所造成的影响, left +1

那么之后遇到 小于 target, 说明 这个范围是合法的, 因为这个范围是合法的, 这个范围中所有连续子数组都是合法的

~~~
例子
[1,2,3,4,5,6,7] 100
[1] 是一个合法范围 他有一个子数组
接下来按照算法 是 [1, 2] 他也是一个合法范围 在这里我们考虑以仅考虑右指针带来的子数组(避免重复) 应该有 [1,2] [2] 两个
接下来按照算法 是 [1, 2, 3] 他也是一个合法范围 他以考虑右指针的子数组是 [1, 2, 3], [2, 3], [3] 三个
...
由此我们可以推导出 仅考虑右指针的连续子数组个数应该 为 right - left + 1
~~~

~~~python
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        DEBUG = 0
        
        if not nums or k <= 1:
            return 0
        
        result = 0
        current = 1
        left, right = 0, 0
        
        while right < len(nums):
            current *= nums[right]
            
            while current >= k and left <= right:
                current /= nums[left]
                left += 1
                
            if DEBUG:
                print('-----------------------')
                print(right - left + 1)
                print('-----------------------')
            result += right - left + 1
            right += 1
        return result
                
~~~





#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)

难度中等902

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**异位词** 指由相同字母重排列形成的字符串（包括相同的字符串）。

 

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 

**提示:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

## 题解

目前只会了 思路1

这里使用***滑动数组***

第一种情况 如果 p 长度 大于了 s 长度, 说明一定不可能

然后分别为 p 和 s 创建两个数组 以 储存两个 字符串中 字母出现的频率

先比较 len(p) 个字符: 这里也做了 p 中字母出现的频率

如果这时候 两个 数组 相同 说明 以 0 开始的 是一个 异位词

接下来就可以继续判断 保证滑动窗口大小不变 向左移动一个

~~~python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        DEBUG = 1
        
        n = len(s)
        m = len(p)
        
        list1 = [0 for i in range(26)]
        list2 = [0 for i in range(26)]
        
        result = []
        
        if m > n:
            return result 
        
        for i in range(m):
            list1[ord(p[i]) - ord('a')] += 1
            list2[ord(s[i]) - ord('a')] += 1
        
        if list1 == list2:
            result.append(0)
            
        for i in range(m, n):
            list2[ord(s[i - m]) - ord('a')] -= 1
            list2[ord(s[i]) - ord('a')] += 1
            
            if list2 == list1:
                result.append(i - m + 1)
        return result
            
        
~~~

