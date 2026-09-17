# 字符串学习心得

本目录用于保存字符串专题的实现代码，并记录算法思路、易错点、边界条件和个人复盘。

## 学习内容

- [x] 字符串理论基础
- [x] 反转字符串
- [x] 替换空格
- [x] 反转字符串中的单词
- [x] 左旋转字符串
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

#### 反转字符串中的单词

![image-20260916111356274](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20260916111356274.png)

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s[::-1] #反转整个字符串
        return ' '.join(word[::-1] for word in s.split())
```

```python
word[::-1] for word in s.split()
#这是一个生成式表达式，含义是：
#s.split()：把字符串 s 按空格切分成单词列表
#word[::-1]：使用切片，将每个单词倒序。其中，[::-1] 可以理解为：[开始位置:结束位置:步长]
#for word in ...：依次遍历每个单词。
```

##### 分割法+双指针

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        s = s.split() #将字符串拆分为单词，里面的空格全部会被去除
        #反转单词
        left , right = 0 , len(s) - 1
        while left < right:
            s[left] , s[right] = s[right] , s[left]
            left += 1
            right -= 1
        return ' '.join(s)
```

###### split函数用法

```python
字符串.split(分隔符, 最大切分次数)
1.不指定分隔符（不指定分隔符时，默认按照空格、多个空格、换行等空白字符切分）
s = "I love Python"
result = s.split()
print(result)#['I', 'love', 'Python']
2.指定分隔符（分隔符会被去掉）
s = "apple,banana,orange"
result = s.split(',')
print(result)#['apple', 'banana', 'orange']
3.指定切分次数（num表示最多切分num次）
s = "a-b-c-d"
result = s.split('-', 2)
print(result)#['a', 'b', 'c-d']
4.与join()配合使用
s = "hello world python"
words = s.split()
result = "-".join(words)
print(result)#hello-world-python
```

#### 左旋转字符串

[动态口令](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/)

![左旋转字符串学习截图](assets/left-rotate-string.png)

```python
class Solution:
    def myreverse(self , start , end , s):
        left , right = start , end-1
        while left < right:
            s[left] , s[right] = s[right] , s[left]
            left += 1
            right -= 1
        return s
    def dynamicPassword(self, password: str, target: int) -> str:
        #1.反转前target-1个字符
        #2，反转第target个到末尾的字符
        #3.反转全部字符
        password = list(password)
        password = self.myreverse(0 , target , password)
        password = self.myreverse(target , len(password) , password)
        password = self.myreverse(0 , len(password) , password)
        return ''.join(password)
```

##### 更简单的方法

```python
class Solution:
    def dynamicPassword(self, password: str, target: int) -> str:
        return password[target:] + password[:target]
```

#### 右旋转字符串

[右旋转字符串](https://kamacoder.com/problempage.php?pid=1065)

![右旋转字符串学习截图](assets/right-rotate-string.png)

```python
#获取输入的数字k和字符串
k = int(input())
s = input()

print(s[-k:] + s[:-k])
```

其中-k的意思是从字符串末尾反向计算位置

例如：

`s[-k:]`：从倒数第 `k` 个字符取到末尾；

`s[:-k]`：从开头取到倒数第 `k` 个字符之前；

负号表示从右往左数。

#### 实现strStr（）

[找出字符串中第一个匹配的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)

![strStr KMP 学习截图](assets/strstr-kmp.png)


```python
class Solution:
    def getNext(self , next_array , s):
        #初始化next数组
        next_array[0]=0
        j = 0#前缀末尾
        i= 0#后缀末尾
        for i in range(1 , len(s)):
            #s[i]与s[j]不相等的情况
            while j > 0 and s[i] != s[j]:#j不能等于0，等于0的话就没有意义了，会卡死验证程序
                j = next_array[j - 1]
            if s[i] == s[j]:
                j += 1
            next_array[i] = j
    def strStr(self, haystack: str, needle: str) -> int:
        #题目说1 <= needle.length <= 10^4,所以不用考虑needle长度为0的情况
        #初始化next数组，长度和子串长度一样
        next_array = [0]*len(needle)
        #获取子串的next数组
        self.getNext(next_array , needle)
        j = 0#子串指针
        i = 0
        while i < len(haystack):
            while j > 0 and haystack[i] != needle[j]:
                j = next_array[j - 1]
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):
                return i - j + 1
            i += 1
        return -1
```
