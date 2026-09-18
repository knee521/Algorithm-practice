# Stock 学习心得

本目录用于记录股票类动态规划问题的实现代码、解题思路、易错点和复盘结论。

## 学习内容

- [x] 用栈实现队列
- [ ] 用队列实现栈
- [ ] 买卖股票的最佳时机
- [ ] 买卖股票的最佳时机 II
- [ ] 含冷冻期的股票买卖
- [ ] 含手续费的股票买卖
- [ ] 股票买卖系列综合复盘

## 核心思路

- 用持有股票和不持有股票表示每天的状态，分别记录两种状态下的最大收益。
- 根据题目限制增加冷冻期、交易次数或手续费等状态，并明确每天的状态转移顺序。
- 先写出状态定义和转移方程，再根据依赖关系压缩数组空间。

## 易错点与边界条件

> 记录交易次数、是否持股、冷冻期、手续费、买入卖出顺序，以及空数组和单日价格等边界情况。

## 复盘

> 每道题记录状态定义、转移方程、初始化、遍历顺序、空间优化和复杂度分析。

## 代码记录

> 在这里补充每道股票算法题的题目链接、实现代码和个人学习笔记。

#### 用栈实现队列

[用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/description/)

![用栈实现队列学习截图](assets/stack-to-queue.png)

```python
class MyQueue:
#队列是先进先出，栈是先进后出
    def __init__(self):
        self.stock_in = [] #进栈
        self.stock_out = [] #出栈

    def push(self, x: int) -> None:
        self.stock_in.append(x)#把元素加进来只需要把x push进来

    def pop(self) -> int:
        if self.stock_out:
            return self.stock_out.pop()
        else:
            for i in range(len(self.stock_in)):
                self.stock_out.append(self.stock_in.pop())
            return self.stock_out.pop()

    def peek(self) -> int:
        res = self.pop()
        self.stock_out.append(res)
        return res

    def empty(self) -> bool:
        if len(self.stock_in) !=0 or len(self.stock_out) != 0:
            return False
        else:
            return True
```

#### 用队列实现栈

[用队列实现栈](https://programmercarl.com/algo/stack-queue/0225-implement-stack-using-queues.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC)

![用队列实现栈学习截图](assets/queue-to-stack.png)

```python
#deque() 是 Python 中的“双端队列”
#它支持在队列的两端快速添加和删除元素
#1.创建deque队列
queue = deque()
#2.从右端添加元素
queue.append(1)
queue.append(2)
print(queue)#deque([1,2])
#3.从左端添加元素
queue.appendleft(0)
print(queue)#deque([0,1,2])
#4.从右端删除元素
queue.pop()#删除并返回右边元素2
#5.从左端删除元素
queue.popleft()#删除并返回左边元素0
```

```python
class MyStack:

    def __init__(self):
        self.que = deque()#定义一个两端都可以操作的数据结构

    def push(self, x: int) -> None:
        self.que.append(x) #从右端加进去

    def pop(self) -> int:
        for i in range(len(self.que) - 1):#不包含最右端元素
            self.que.append(self.que.popleft())
        return self.que.popleft()

    def top(self) -> int:
        #违反题目要求：只能使用队列的标准操作
        #return self.que[-1]
        #1.先把除了栈顶的元素全部移动到栈顶的右边
        for i in range(len(self.que) - 1):
            self.que.append(self.que.popleft())
        temp = self.que.popleft()
        self.que.append(temp)
        return temp

    def empty(self) -> bool:
        return not self.que
```
