## 华为 Leetcode 字符串

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

难度中等7658

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成



## 题解

这里的思路主要是 ***滑动窗口***

如果当前 字符不会导致重复加入, 如果当前字符会导致重复, 从左到右删除直至删除倒置重复的字符, 并把当前字符加入. 并且每一步操作, 需要去比较当前字符串长度 和 最长长度

~~~python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        DEBUG = 1
        ## 如果 s 长度小于 2
        n = len(s)
        if n < 2:
            return n
        current = "".join(s[0])
        maxmum = 0
        for i in range(1, len(s)):
            print(current)
            if s[i] in current:
                maxmum = max(maxmum, len(current))
                ## 然后要找到之前重复的
                for j in range(len(current)):
                    if current[j] == s[i]:
                        if j + 1 <= len(current):
                            current = current[j + 1:] + s[i]
                        else:
                            current = s[i]
                        break
            else:
                current += s[i]
        return max(maxmum, len(current))
~~~





#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

难度中等926

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 

**提示：**

- `1 <= s.length <= 20`
- `s` 仅由数字组成





## 题解

因为涉及到排列组合, 所以这里使用 ***回溯***



~~~python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        DEBUG = 0
        n = len(s)
        result = []
        segments = [0 for i in range(4)]
        ## 首先如果 s 的长度 小于4 或者大于 12 就一定没结果
        if n < 4 or n > 12:
            print('here')
            return result

        ## 回溯
        def backtrack(lev, start):
            ## 合法输出, 分成四段, 且用完所有的数字
            if lev == 4 and start == n:
                result.append('.'.join(str(seg) for seg in segments))
            ## 这时候到这的一定是不合法的
            if lev == 4 or start == n:
                return 
            ## 首数字是0
            if s[start] == "0":
                segments[lev] = 0
                backtrack(lev + 1, start + 1)
            else:
                ## 剩下的都是正常情况
                temp = ''
                for i in range(start, n):
                    temp += s[i]
                    curr = int(temp)
                    if 0 <= curr <= 255:
                        segments[lev] = curr
                        backtrack(lev + 1, i + 1)
                    else:
                        break
            return
        backtrack(0, 0)
        return result
~~~

