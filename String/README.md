# 字符串学习心得

本目录用于保存字符串专题的实现代码，并记录算法思路、易错点、边界条件和个人复盘。

## 学习内容

- [x] 字符串理论基础
- [x] 反转字符串
- [x] 替换空格
- [x] 反转字符串中的单词
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

#### 反转字符串Ⅱ

[反转字符串Ⅱ](https://leetcode.cn/problems/reverse-string-ii/description/)

![反转字符串 II 学习截图](assets/reverse-string-ii.png)

```
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        #先写一个函数用来把数组内的元素全部反转
        def myreverse(chars):
            left , right = 0 , len(chars) - 1
            while left < right:
                chars[left] , chars[right] = chars[right] , chars[left]
                left += 1
                right -= 1
            return chars
        res = list(s)#因为 Python 中的字符串 str 是不可变对象，不能直接修改其中某个位置的字符
        for cur in range(0 , len(s) , 2 * k):
            res[cur : cur + k] = myreverse(res[cur : cur + k])
        return ''.join(res)
```

分隔符.join(字符串序列)

eg：res = ['我', '爱', '你']  print(''.join(res))  结果是：我爱你

res = ['I', 'love', 'you'] print(' '.join(res))  结果是：I love you

#### 替换空格

![替换空格学习截图](assets/replace-space.png)

```python
class Solution:
    def pathEncryption(self, path: str) -> str:
        res = list(path)
        for i in range(len(res)):
            if res[i] == '.':
                res[i] = ' '
        return ''.join(res)
```
