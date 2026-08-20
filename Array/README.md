# 数组学习心得

本目录用于保存数组专题的实现代码，并持续记录个人学习心得。

## 今日资料

- 《代码随想录》数组（V3.0）
- 本地资料：`D:\work\算法pdf\1.《代码随想录》数组（V3.0）.pdf`

## 学习内容

- [x] 数组理论基础
- [x] 二分查找
- [ ] 移除元素
- [ ] 有序数组的平方
- [ ] 长度最小的子数组
- [ ] 螺旋矩阵 II
- [ ] 数组总结

## 学习心得

### 核心思路

> #### 数组理论基础
>
> 数组是存储在连续内存空间的相同类型数据的集合。
>
> ##### 需要注意数组元素不能删除，只能覆盖
>
> 

### 易错点与边界条件

> 1. 二分查找有两种方法，一个是左闭右闭区间法，一个是左闭右开区间法，习惯使用左闭右闭区间法，left可以等于right，下一步比较的时候不用包含middle，即right=middle-1或left=middle+1，该方法时间复杂度O(log n),空间复杂度O(1)

### 复杂度总结

> 在这里记录各实现的时间复杂度、空间复杂度及其取舍。

### 复盘

> 在这里记录独立完成情况、需要重做的题目与下一步计划。

## 力扣代码

每道题可单独建立一个目录或源文件，建议使用 `题号-题目名称` 命名，并在代码中注明思路和复杂度。

#### 二分查找

1. [二分查找](https://leetcode.cn/problems/binary-search/)

   ![二分查找通过记录](./assets/binary-search-accepted.png)

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        while left <= right:
            middle = left + (right - left) // 2
            if nums[middle] > target:
                right = middle - 1
            elif nums[middle] < target:
                left = middle + 1
            else: 
                return middle
        return -1
```

