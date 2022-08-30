#### [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

难度简单340

请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

 

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

 

**限制：**

```
0 <= s 的长度 <= 10000
```





## 题解

简单模拟

~~~python
class Solution:
    def replaceSpace(self, s: str) -> str:
        DEBUG = 0
        lst = list(s)
        for i in range(len(lst)):
            if lst[i] == " ":
                lst[i] = "%20"
        return "".join(lst)
       
~~~

