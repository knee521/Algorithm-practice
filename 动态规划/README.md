# 动态规划学习心得

本目录用于保存动态规划专题的实现代码，并记录状态定义、递推公式、初始化、遍历顺序、易错点和个人复盘。

## 学习内容

- [x] 动态规划基础与解题步骤
- [x] 基础递推（斐波那契数、爬楼梯等）
- [x] 路径问题（不同路径等）
- [x] 01背包与完全背包
- [ ] 打家劫舍问题
- [ ] 股票买卖问题
- [ ] 子序列与子串问题
- [ ] 状态压缩与空间优化
- [ ] 动态规划综合复盘

## 核心思路

1. 明确 `dp` 数组及下标的含义，说明每个状态代表什么。
2. 根据状态之间的依赖关系推导递推公式。
3. 确定初始状态，以及不可达状态的表示方式。
4. 确定遍历顺序，保证计算当前状态时所需的状态已经得到。
5. 用小样例手动推导 `dp` 的变化，核对最终答案的位置。

## 易错点与边界条件

- **状态含义一致**：区分“前 i 个元素”和“以下标 i 结尾”，避免递推公式与答案位置不匹配。
- **初始化不能套用**：求方案数、最大值、最小值和可行性时，初值不同；尤其要区分“恰好装满”和“容量不超过上限”。
- **遍历顺序影响结果**：一维01背包通常倒序遍历容量，避免重复选同一物品；一维完全背包通常正序遍历容量，允许重复选取。计数题还要区分组合与排列，依据状态含义决定内外层循环。
- **空间压缩前先理清依赖**：滚动数组会覆盖旧值，更新前确认需要的是上一轮还是本轮状态，必要时用临时变量保存。
- **子序列与子串不同**：子序列允许不连续，子串要求连续，不能直接复用同一递推公式。
- **边界检查**：按题目约束检查空输入、单元素、容量或目标为0、无解、障碍及全负数等情况，避免越界或将不可达状态当成有效答案。
- **Python二维数组**：使用 `[[0] * cols for _ in range(rows)]` 初始化，避免 `[[0] * cols] * rows` 造成多行引用同一列表。

## 复盘

> 后续根据每道题的注释和笔记，总结状态定义、递推公式、初始化、遍历顺序和实际遇到的问题，并记录时间与空间复杂度。

## 代码记录

#### 斐波那契数

[斐波那契数](https://leetcode.cn/problems/fibonacci-number/description/)

![斐波那契数学习截图](assets/fibonacci-number.png)

```python
class Solution:
    def fib(self, n: int) -> int:
        #排除个例
        if n <=1:return n
        #创建dp数组
        dp = [0 , 1]
        ## 遍历顺序: 由前向后。因为后面要用到前面的状态
        for i in range(2 , n+1):
            total = dp[1] + dp[0]
            dp[0] = dp[1]
            dp[1] = total
        return dp[1]
```

#### 爬楼梯

[爬楼梯](https://leetcode.cn/problems/climbing-stairs/description/)

![爬楼梯学习截图](assets/climbing-stairs.png)

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 1:return n
        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 2
        for i in range(3 , n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
        return dp[n]
```

#### 使用最小花费爬楼梯

[使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

![使用最小花费爬楼梯学习截图](assets/min-cost-climbing-stairs.png)

```python
class Solution:
    def minCostClimbingStairs(self, cost: list[int]) -> int:
        #初始化dp数组
        dp = [0] * (len(cost) + 1)
        dp[0] = 0
        dp[1] = 0#初始起点不花费体力值
        for i in range(2 , len(cost) + 1):
            dp[i] = min(dp[i - 1] + cost[i - 1] , dp[i - 2] + cost[i - 2])
        return dp[len(cost)]
```

  #### 不同路径

[不同路径](https://leetcode.cn/problems/unique-paths/description/)

![不同路径学习截图](assets/unique-paths.png)

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        #初始化二维数组
        dp = [[0] * n for _ in range(m)]
        #第一行第一列设为1
        for i in range(n):
            dp[0][i] = 1
        for j in range(m):
            dp[j][0] = 1
        #计算每个单元格的路径数
        for i in range(1 , m):
            for j in range(1 , n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
        return dp[m-1][n-1]
```

#### 不同路径Ⅱ

[不同路径Ⅱ](https://leetcode.cn/problems/unique-paths-ii/description/)

![不同路径Ⅱ学习截图](assets/unique-paths-ii.png)

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: list[list[int]]) -> int:
        #获取棋盘大小
        m = len(obstacleGrid)#行
        n = len(obstacleGrid[0])#列
        #初始化dp数组
        dp = [[0] * n for _ in range(m)]
        #初始化第一行第一列
        for i in range(n):
            if obstacleGrid[0][i] == 1 :
                break
            else:
                dp[0][i] = 1
        for j in range(m):
            if obstacleGrid[j][0] == 1:
                break
            else:
                dp[j][0] = 1
        #遍历棋盘
        for i in range(1 , m):
            for j in range(1 , n):
                if obstacleGrid[i][j] == 1:
                    continue
                else:
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
        return dp[m-1][n-1]
```

#### 整数拆分

[整数拆分](https://leetcode.cn/problems/integer-break/description/)

![整数拆分学习截图](assets/integer-break.png)

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        # 假设对正整数 i 拆分出的第一个正整数是 j（1 <= j < i），则有以下两种方案：
        # 1) 将 i 拆分成 j 和 i−j 的和，且 i−j 不再拆分成多个正整数，此时的乘积是 j * (i-j)
        # 2) 将 i 拆分成 j 和 i−j 的和，且 i−j 继续拆分成多个正整数，此时的乘积是 j * dp[i-j]
        dp = [0] * (n + 1)#dp[i] 表示“整数 i 拆分后能得到的最大乘积”，所以必须能访问到 dp[n],python数组下标从0开始，所以要定义dp数组长度为n+1
        dp[2] = 1
        for i in range(3 , n + 1):
            for j in range(1 , n // 2 + 1):
                dp[i] = max(dp[i] , j * (i - j) , j * dp[i - j])
        return dp[n]

```

#### 不同的二叉搜索树

[不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/description/)

![不同的二叉搜索树学习截图](assets/unique-binary-search-trees.png)

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0] * (n + 1)
        dp[0] = 1
        for i in range(1 , n+1):
            for j in range(1 , i + 1):
                dp[i] += dp[j - 1] * dp[i - j]#j - 1是以j为头节点的左子树的节点数量，i-j是以j为头节点的右子树的节点数量
        return dp[n]
```

#### 背包理论基础一

[背包理论基础一](https://kamacoder.com/problempage.php?pid=1046)

![背包理论基础一学习截图](assets/knapsack-basics-1.png)

```python
n, bagweight = map(int, input().split())

weight = list(map(int, input().split()))
value = list(map(int, input().split()))

dp = [[0] * (bagweight + 1) for _ in range(n)]
for i in range(weight[0] , bagweight + 1):#从物品0的重量开始遍历到书包的重量结束
    dp[0][i] = value[0]

for i in range(1 , n):
    for j in range(bagweight + 1):#从重量0开始遍历到书包重量结束
        if j < weight[i]:#如果小于物品i的重量，那么此时书包的价值是上一个物品使用此重量书包的价值
            dp[i][j] = dp[i-1][j]
        else:
            dp[i][j] = max(dp[i-1][j] , dp[i-1][j - weight[i]] + value[i])
print(dp[n - 1][bagweight])

```

#### 分割等和子集

[分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

![分割等和子集学习截图](assets/partition-equal-subset-sum.png)

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        #物品是nums[i]，重量是nums[i]，价值也是nums[i]，背包体积是sum/2
        total_sum = sum(nums)
        if total_sum % 2 != 0: return False#nums的总和必须是2的倍数
        target = total_sum // 2
        dp = [0] * (target + 1)
        for i in nums:
            for j in range(target , i -1 , -1):
                dp[j] = max(dp[j] , dp[j - i] + i)
        return dp[-1] == target
```

#### 最后一块石头的重量Ⅱ

[最后一块石头的重量Ⅱ](https://leetcode.cn/problems/last-stone-weight-ii/description/)

![最后一块石头的重量Ⅱ学习截图](assets/last-stone-weight-ii.png)

```python
class Solution:
    def lastStoneWeightII(self, stones: list[int]) -> int:
        dp = [0] * 15000
        total_sum = sum(stones)
        target = total_sum // 2
        for i in stones:
            for j in range(target , i - 1, -1):#i-1是为了让j可以取到i
                dp[j] = max(dp[j] , dp[j - i] + i)
        return total_sum - 2*dp[target]
```

#### 目标和

[目标和](https://leetcode.cn/problems/target-sum/description/)

![目标和学习截图](assets/target-sum.png)

```python
class Solution:
    def findTargetSumWays(self, nums: list[int], target: int) -> int:
        total_sum = sum(nums)
        if total_sum < abs(target):
            return 0
        if (total_sum + target) % 2 != 0:
            return 0
        #目标和
        target_sum = (total_sum + target) // 2
        dp = [0] * (target_sum + 1)
        dp[0] = 1
        for i in nums:
            for j in range(target_sum , i - 1 , -1):
                dp[j] += dp[j-i]
        return dp[target_sum]
```

#### 一和零

[一和零](https://leetcode.cn/problems/ones-and-zeroes/)

![一和零学习截图](assets/ones-and-zeroes.png)

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m+1)]
        #统计0、1个数
        for s in strs:
            zeroNum = s.count('0')
            oneNum = len(s) - zeroNum
            for i in range(m , zeroNum - 1 , -1):
                for j in range(n , oneNum - 1 , -1):
                    dp[i][j] = max(dp[i][j] , dp[i - zeroNum][j - oneNum] + 1)
        return dp[m][n]
```

#### 完全背包

[完全背包](https://kamacoder.com/problempage.php?pid=1052)

![完全背包学习截图](assets/complete-knapsack.png)

```python
def knapsack(n, bag_weight, weight, value):
    dp = [[0] * (bag_weight + 1) for _ in range(n)]#背包重量加1是要把重量为0考虑进去
    #初始化dp数组
    for j in range(weight[0] , bag_weight + 1):
        dp[0][j] = dp[0][j - weight[0]]+value[0]#dp数组第一行只要还能加物品0，就继续加
    #动态规划,先遍历物品种类，再遍历背包重量
    for i in range(1 , n):
        for j in range(bag_weight + 1):
            if j < weight[i]:dp[i][j] = dp[i-1][j]
            else:
                dp[i][j] = max(dp[i-1][j] , dp[i][j - weight[i]] + value[i])
    return dp[n-1][bag_weight]

#输入
n , bag_weight = map(int , input().split())
weight = []
value = []
for _ in range(n):
    w , v = map(int , input().split())
    weight.append(w)
    value.append(v)
print(knapsack(n , bag_weight, weight, value))
```

#### 零钱兑换Ⅱ

[零钱兑换Ⅱ](https://leetcode.cn/problems/coin-change-ii/description/)

![零钱兑换Ⅱ学习截图](assets/coin-change-ii.png)

```python
class Solution:
    def change(self, amount: int, coins: list[int]) -> int:
        dp = [0] * (amount + 1)
        dp[0] = 1#背包重量为0的组合种类有1个
        #这个完全背包要考虑顺序问题，所以外层遍历种类，内层遍历背包重量
        for i in range(len(coins)):
            for j in range(coins[i] , amount + 1):#当前硬币面值是 coins[i]，只有金额 j >= coins[i] 时，才放得下这枚硬币，才能用它凑钱。
                dp[j] += dp[j - coins[i]]
        return dp[amount]
```

#### 组合总和Ⅳ

[组合总和Ⅳ](https://leetcode.cn/problems/combination-sum-iv/description/)

![组合总和Ⅳ学习截图](assets/combination-sum-iv.png)

```python
class Solution:
    def combinationSum4(self, nums: list[int], target: int) -> int:
        #如果求组合数就是外层for循环遍历物品，内层for遍历背包。
        #如果求排列数就是外层for遍历背包，内层for循环遍历物品。
        dp = [0] * (target + 1)
        dp[0] = 1
        for i in range(target  + 1):
            for j in range(len(nums)):
                if i >= nums[j]:
                    dp[i] += dp[i - nums[j]]
        return dp[target]
```

#### 爬楼梯进阶版

[爬楼梯进阶版](https://kamacoder.com/problempage.php?pid=1067)

![爬楼梯进阶版学习截图](assets/climbing-stairs-advanced.png)

```python
def climbing_stairs(n,m):
    #背包总容量
    dp = [0] * (n + 1)
    dp[0] = 1
    #排列题
    for j in range(1 , n + 1):
        for i in range(1 , m + 1):
            if j >=  i:
                dp[j] += dp[j - i]
    return dp[n]

if __name__ == '__main__':
    n , m = list(map(int , input().split(' ')))
    print(climbing_stairs(n , m))
```
