# 链表学习心得

本目录用于保存链表专题的实现代码，并持续记录个人学习心得。

## 今日资料

- 《代码随想录》链表（V3.0）
- 本地资料：`D:\work\算法pdf\2.《代码随想录》链表.（V3.0）.pdf`

## 学习内容

- [x] 链表理论基础
- [ ] 移除链表元素
- [ ] 设计链表
- [ ] 翻转链表
- [ ] 两两交换链表中的节点
- [ ] 删除链表的倒数第 N 个节点
- [ ] 链表相交
- [ ] 环形链表 II
- [ ] 链表总结

## 学习心得

### 核心思路

> - 链表定义
>
>   ```python
>   # 1. 定义节点 
>   class ListNode: 
>       def __init__(self, val=0, next=None): 
>           self.val = val       # 节点存储的值 
>           self.next = next     # 下一个节点，默认为 None 
>   # 2. 创建链表：1 → 2 → 3 
>   head = ListNode(1) 
>   head.next = ListNode(2) 
>   head.next.next = ListNode(3) 
>   # 3. 遍历链表 
>   current = head 
>   while current is not None: 
>       print(current.val) 
>       current = current.next
>   ```
>
> - 删除链表中的元素
>
>   设置一个虚拟头结点，这样链表中所有元素都可以按照同一个方式进行移除

### 易错点与边界条件

> 在这里记录空链表、单节点、头节点变化、指针越界和成环等易错情况。

### 复杂度总结

> 在这里记录各实现的时间复杂度、空间复杂度及其取舍。

### 复盘

> 在这里逐题记录遇到的问题、错误原因、修正方法和需要重做的内容。

## 力扣代码

1. [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)

![移除链表元素通过记录](./assets/remove-linked-list-elements-accepted.png)

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    #Optional的意思是创建的head可以是指定的ListNode类型，也可以是None
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummyhead = ListNode(0)#定义一个虚拟的头结点
        dummyhead.next = head #指向链表的头节点
        cur = dummyhead#用来遍历链表
        while  cur.next is not None:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next
        head = dummyhead.next
        return head
```

head: Optional[ListNode]，意思是定义的head可以是None也可以是ListNode，他通常会把类型标注和赋值一起写：

```python
定义空链表：head: Optional[ListNode] = None
创建头节点：head: Optional[ListNode] = ListNode(1)
```

