# 字符串学习心得

本目录用于保存字符串专题的实现代码，并记录算法思路、易错点、边界条件和个人复盘。

## 学习内容

- [x] 字符串理论基础
- [x] 反转字符串
- [ ] 反转字符串中的单词
- [ ] 右旋转字符串
- [ ] 实现 strStr()
- [ ] 重复的子字符串

## 核心思路

- 根据题意选择遍历、双指针、滑动窗口、栈或字符串匹配算法。
- 处理字符时明确区分字符、字符串和下标，必要时先转换为列表再原地修改。
- 涉及空格、大小写或连续分隔符时，先明确题目要求的保留和删除规则。

## 易错点与边界条件

> 在这里记录空字符串、单字符、全空格、连续空格、大小写、特殊字符、下标越界和原地修改等问题。

## 复盘

> 每道题记录遇到的问题、错误原因、修正方法、复杂度分析和需要重做的内容。

## 代码记录

#### 反转字符串

[反转字符串](https://leetcode.cn/problems/reverse-string/description/)

![反转字符串学习截图](assets/reverse-string.png)

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left , right = 0 , len(s) - 1
        while left <= right:
            s[left] , s[right] = s[right] , s[left]
            left += 1
            right -= 1
```

s[left] , s[right] = s[right] , s[left]是python特有的，直接交换两个数
