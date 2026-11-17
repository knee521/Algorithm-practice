# 单调栈学习心得

本目录用于保存单调栈专题的实现代码，记录栈的维护规则、易错点、边界条件和个人复盘。

## 学习内容

- [ ] 单调栈基础与进出栈规则
- [ ] 下一个更大元素（每日温度、下一个更大元素Ⅰ与Ⅱ）
- [ ] 栈顶两侧的边界（接雨水、柱状图中最大的矩形）
- [ ] 单调栈综合复盘

## 核心思路

- 先确定要找左侧还是右侧、严格更大还是严格更小，再据此决定遍历方向和弹栈条件。
- 以从左向右寻找右侧第一个严格更大的元素为例，栈保存尚未找到答案的下标，对应值从栈底到栈顶非递增。当前值大于栈顶对应值时，弹出栈顶并记录答案，直到条件不再成立，再压入当前下标。
- 常规单次扫描中，每个下标最多入栈、出栈各一次，时间复杂度为 `O(n)`，栈的额外空间为 `O(n)`。

## 易错点与边界条件

- **相等值**。严格更大与大于等于的弹栈规则不同。柱状图遇到等高柱时，要明确保留哪个下标并统一计算边界。
- **值和下标**。需要算等待天数或区间宽度时，栈里保留下标，通过原数组读取对应值。
- **循环数组**。可扫描两轮并用取模访问原下标，第一轮压入待求解下标，第二轮继续处理未解项。
- **未找到边界**。按题意初始化默认结果，比如每日温度填 `0`、下一个更大元素填 `-1`。注意全相等、单调序列和单元素输入。

## 复盘

1. **每日温度**。栈里存还没找到更暖一天的下标，温度从栈底到栈顶非递增。当前温度严格大于栈顶温度时，当前下标就是栈顶右侧第一个更暖的日子，等待天数为 `i - stack[-1]`。一次扫描可能解出多个位置，所以用 `while` 连续弹栈。相同温度继续等待，最终没找到答案的位置保留 `0`。
2. **下一个更大的元素Ⅰ**。栈里存 `nums2` 的下标，弹栈时用该下标取出原值，再找到它在 `nums1` 中的位置，把当前元素的值写进答案。没有更大元素的位置保留 `-1`。笔记中的 `in nums1` 和 `nums1.index(...)` 都会线性查找，整体最坏为 `O(mn)`，其中 `m`、`n` 分别是两个数组的长度。题目中的值互不重复，可以预先建立“值 → nums1 下标”的字典，把总时间降到平均 `O(m+n)`。
3. **下一个更大元素Ⅱ**。用 `i % len(nums)` 把第二轮扫描映射回原下标，给数组末尾尚未找到答案的元素继续向开头查找。继续用严格大于弹栈，避免把自己或相等值当作答案。笔记里的写法两轮都入栈，仍为 `O(n)` 时间和空间。第一轮入栈、第二轮只弹栈也能完成匹配，并省下重复入栈。
4. **接雨水**。从栈底到栈顶保持高度非递增。遇到更高的右边界时，弹出的柱子是凹槽底，新栈顶是左边界。栈弹空时缺少左边界，不能计水。按层累加 `(min(左高, 右高) - 底高) * (右下标 - 左下标 - 1)`。当前柱与栈顶等高时可用靠右的下标替换栈顶。小于、等于、大于三种情况可以写成互斥分支，避免入栈后又与当前下标自身比较。
5. **柱状图中最大的矩形**。从栈底到栈顶保持高度非递减。遇到更矮的柱子时，弹出的柱子提供矩形高度，当前下标是右边界，用弹出后的栈顶与当前下标计算宽度 `右下标 - 左下标 - 1`，每次取面积最大值。等高柱可以由靠右的一根继续代表，后续仍能覆盖这一段。首尾补 `0` 分别提供左侧哨兵和触发最后一批正高度柱子出栈。笔记使用 `insert` 和 `append`，会改变传入的列表。

对照两道柱子题，宽度都排除左右边界。接雨水找更高的柱子形成右边界，计算每一层的水量并累加。柱状图遇到更矮的柱子就结算可延伸的矩形面积，并取最大值。两种写法都只线性扫描并处理入栈、出栈，时间和额外空间均为 `O(n)`。

## 代码记录

#### 每日温度

[每日温度](https://leetcode.cn/problems/daily-temperatures/description/)

![每日温度学习截图](assets/daily-temperatures.png)

```python
class Solution:
    def dailyTemperatures(self, temperatures: list[int]) -> list[int]:
        result = [0] * (len(temperatures))
        stack = [0]
        for i in range(1 , len(temperatures)):
            if temperatures[i] <= temperatures[stack[-1]]:
                stack.append(i)
            else:
                while len(stack) != 0 and temperatures[i] > temperatures[stack[-1]]:
                    result[stack[-1]] = i - stack[-1]
                    stack.pop()
                stack.append(i)
        return result

        ##如果当前遍历的元素 大于栈顶元素，表示 栈顶元素的 右边的最大的元素就是 当前遍历的元素，
        ##所以弹出 栈顶元素，并记录
        ##如果栈不空的话，还要考虑新的栈顶与当前元素的大小关系
        ##否则的话，可以直接入栈。
        ##注意，单调栈里 加入的元素是 下标。
```

#### 下一个更大的元素Ⅰ

[下一个更大的元素Ⅰ](https://leetcode.cn/problems/next-greater-element-i/description/)

![下一个更大的元素Ⅰ学习截图](assets/next-greater-element-i.png)

```python
class Solution:
    def nextGreaterElement(self, nums1: list[int], nums2: list[int]) -> list[int]:
        result = [-1] * (len(nums1))
        stack = [0]
        for i in range(1 , len(nums2)):
            if nums2[i] <= nums2[stack[-1]]:
                stack.append(i)
            else:
                while len(stack) != 0 and nums2[i] > nums2[stack[-1]]:
                    if nums2[stack[-1]] in nums1:
                        index = nums1.index(nums2[stack[-1]])#nums1.index 是方法，要用圆括号调用
                        result[index] = nums2[i]#栈里始终存 nums2 的下标，所以要压入 i。
                    stack.pop()
                stack.append(i)
        return result
```

#### 下一个更大元素Ⅱ

[下一个更大元素Ⅱ](https://leetcode.cn/problems/next-greater-element-ii/description/)

![下一个更大元素Ⅱ学习截图](assets/next-greater-element-ii.png)

```python
class Solution:
    def nextGreaterElements(self, nums: list[int]) -> list[int]:
        result = [-1] * len(nums)
        stack = [0]
        # 模拟遍历两边nums，注意一下都是用i % nums.size()来操作
        for i in range(1 , len(nums) * 2):
            while (len(stack) != 0) and nums[i % len(nums)] > nums[stack[-1]]:
                result[stack[-1]] = nums[i % len(nums)]
                stack.pop()
            stack.append(i % len(nums))
        return result
```

#### 接雨水

[接雨水](https://leetcode.cn/problems/trapping-rain-water/description/)

![接雨水学习截图](assets/trapping-rain-water.png)

```python
class Solution:
    def trap(self, height: list[int]) -> int:
        '''
        单调栈是按照 行 的方向来计算雨水
        从栈顶到栈底的顺序：从小到大
        通过三个元素来接水：栈顶，栈顶的下一个元素，以及即将入栈的元素
        雨水高度是 min(凹槽左边高度, 凹槽右边高度) - 凹槽底部高度
        雨水的宽度是 凹槽右边的下标 - 凹槽左边的下标 - 1（因为只求中间宽度）
        '''
        result = 0#表示能接多少水
        stack = [0] #存下标
        for i in range(1 , len(height)):
            #1.即将入栈的元素高度小于栈顶元素的高度，直接入栈
            if height[i] < height[stack[-1]]:
                stack.append(i)
            #2.即将入栈的元素高度等于栈顶元素的高度，弹出栈顶元素，加入即将入栈的元素
            if height[i] == height[stack[-1]]:
                stack.pop()
                stack.append(i)
            #3.即将入栈的元素高度大于栈顶元素的高度
            if height[i] > height[stack[-1]]:
                while len(stack) != 0 and height[i] > height[stack[-1]]:
                    mid_height = height[stack[-1]]
                    stack.pop()
                    if stack:
                        left_height = height[stack[-1]]
                        right_height = height[i]
                        h = min(left_height , right_height) - mid_height
                        w = i - stack[-1] - 1
                        result += h * w
                stack.append(i)
        return result
```

#### 柱状图中最大的矩形

[柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)

![柱状图中最大的矩形学习截图](assets/largest-rectangle-in-histogram.png)

```python
class Solution:
    def largestRectangleArea(self, heights: list[int]) -> int:
        '''
        找每个柱子左右侧的第一个高度值小于该柱子的柱子
        单调栈：栈顶到栈底：从大到小（每插入一个新的小数值时，都要弹出先前的大数值）
        栈顶，栈顶的下一个元素，即将入栈的元素：这三个元素组成了最大面积的高度和宽度
        情况一：当前遍历的元素heights[i]大于栈顶元素的情况
        情况二：当前遍历的元素heights[i]等于栈顶元素的情况
        情况三：当前遍历的元素heights[i]小于栈顶元素的情况
        '''
        # 输入数组首尾各补上一个0
        heights.insert(0 , 0)#第一个 0 是插入位置，第二个 0 是插入的值
        heights.append(0)
        stack = [0]
        result = 0
        for i in range(1 , len(heights)):
            #情况一
            if heights[i] > heights[stack[-1]]:
                stack.append(i)
            elif heights[i] == heights[stack[-1]]:
                stack.pop()
                stack.append(i)
            elif heights[i] < heights[stack[-1]]:
                while stack and heights[i] < heights[stack[-1]]:
                    mid_height = heights[stack[-1]]
                    stack.pop()
                    if stack:
                        left_index = stack[-1]
                        right_index = i
                        w = right_index - left_index - 1
                        result = max(result , w * mid_height)
                stack.append(i)

        return result
```
