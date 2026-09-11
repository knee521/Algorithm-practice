# 链表学习心得

本目录用于保存链表专题的实现代码，并持续记录个人学习心得。

## 今日资料

- 《代码随想录》链表（V3.0）
- 本地资料：`D:\work\算法pdf\2.《代码随想录》链表.（V3.0）.pdf`

## 学习内容

- [x] 链表理论基础
- [x] 移除链表元素
- [x] 设计链表
- [x] 翻转链表
- [x] 两两交换链表中的节点
- [x] 删除链表的倒数第 N 个节点
- [x] 链表相交
- [x] 环形链表 II
- [x] 链表总结

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

> - **空链表、单节点**：先判断 `head` 或 `next` 是否为空，避免访问空指针；删除头节点时优先使用虚拟头结点。
> - **下标边界**：`get`/删除要求 `0 <= index < size`，插入允许 `index == size`。
> - **尾指针与长度**：插入、删除后同步更新 `size`；删除尾节点时更新 `tail`。
> - **修改指针**：反转或交换前先保存后继节点，否则链表剩余部分会丢失。
> - **双指针与成环**：快慢指针移动前检查 `fast` 和 `fast.next`；相遇后再定位入环点。
> - **相交判断**：比较节点对象而非节点值；无交点或无环时返回 `None`。



### 复盘

> - **移除链表元素**：用虚拟头结点统一处理头节点和普通节点；删除后当前指针不要立即后移。
> - **设计链表**：核心是维护 `dummyhead`、`size` 和 `tail`，尤其注意插入与删除的边界不同。
> - **反转、交换节点**：本质是保存后继、调整链接、移动指针；双指针循环必须覆盖空链表和奇数节点。
> - **删除倒数第 N 个节点**：快指针先走 `n + 1` 步，使慢指针停在待删节点的前一个位置，可统一处理删除头节点。
> - **链表相交**：先对齐两条链的尾部，再同步比较节点；相交依据是同一节点，不是相同数值。
> - **环形链表 II**：Floyd 法分为“判断相遇”和“寻找入口”两阶段；哈希集合则记录已访问节点，空间换时间。
> - **通用复盘**：每次改链前确认后继是否已保存，循环结束后检查返回的新头结点、尾结点和长度是否正确。

## 力扣代码

#### 移除链表元素

1. [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/description/)

![移除链表元素通过记录](./assets/remove-linked-list-elements-accepted.png)

```python
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

#### 设计链表

2. [707. 设计链表](https://leetcode.cn/problems/design-linked-list/description/)

![设计链表通过记录](./assets/design-linked-list-accepted.png)

```python
class ListNode:
    def __init__(self,val=0,next=None):
        self.val = val 
        self.next = next
class MyLinkedList:

    def __init__(self):
        self.dummyhead = ListNode()
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        cur = self.dummyhead.next #指向链表头节点的整体
        for i in range(index):#index=0时，不移动
            cur = cur.next
        return cur.val

    def addAtHead(self, val: int) -> None:
        temp = ListNode(val)
        temp.next = self.dummyhead.next#现在temp.next指向原先的头节点
        self.dummyhead.next = temp
        self.size += 1

    def addAtTail(self, val: int) -> None:
        temp = ListNode(val)
        cur = self.dummyhead
        while cur.next !=None:
            cur = cur.next
        cur.next = temp
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return None
        cur = self.dummyhead
        while index:
            cur = cur.next
            index -=1
        cur.next = ListNode(val , cur.next)
        self.size +=1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return None
        cur = self.dummyhead
        while index:
            cur = cur.next
            index -=1
        cur.next = cur.next.next
        self.size -=1
```

下面是执行用时优化的版本

```python
class ListNode:
    def __init__(self,val=0,next=None):
        self.val = val 
        self.next = next
class MyLinkedList:

    def __init__(self):
        self.dummyhead = ListNode()
        self.size = 0
        self.tail = self.dummyhead

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        cur = self.dummyhead.next #指向链表头节点的整体
        for _ in range(index):#index=0时，不移动
            cur = cur.next
        return cur.val

    def addAtHead(self, val: int) -> None:
        self.dummyhead.next = ListNode(val,self.dummyhead.next)
        if self.size == 0:
            self.tail = self.dummyhead.next
        self.size += 1

    def addAtTail(self, val: int) -> None:
        self.tail.next = ListNode(val)
        self.tail = self.tail.next
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:#可以在index=size的位置添加结点，但是不能在这个位置删除结点
            return None
        if index == self.size:
            self.addAtTail(val)
            return
        cur = self.dummyhead
        for _ in range(index):
            cur = cur.next
        cur.next = ListNode(val, cur.next)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return None
        cur = self.dummyhead
        for _ in range(index):
            cur = cur.next
        #必须在删除之前判断删除的结点是否是尾结点
        if cur.next == self.tail:
            self.tail = cur
        cur.next = cur.next.next
        self.size -=1
```

#### 反转链表

1. [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)

![反转链表通过记录](./assets/reverse-linked-list-accepted.png)

##### 双指针法

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        temp = None #存储cur的next，也就是cur的下一个结点
        cur = head #cur相当于head
        pre = None #指向最后一个结点
        while cur is not None:
            temp = cur.next #临时存储cur的下一个结点
            cur.next = pre #反转结点指向
            pre = cur #pre指向头节点
            cur = temp #cur再重新指向下一个结点
        return pre
```

##### 递归法

```python
class Solution:
    def reverse(self , cur , pre):
        if cur == None:
            return pre
        temp = cur.next #先保存下一个结点的地址
        cur.next = pre #保存完之后，把当前结点的指针域改为None
        return self.reverse(temp , cur)#开始下一个结点与倒数第二个结点互换

    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        return self.reverse(head , None)
```

#### 两两交换链表中的节点

[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

![两两交换链表中的节点通过记录](./assets/swap-nodes-in-pairs-accepted.png)

```python
class Solution:
    # 1 --> 2 --> 3 --> 4 --> None
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummyNode = ListNode(0 , head) #虚拟头节点指向真实的头节点
        cur = dummyNode #用于后面的删除操作
        while cur.next != None and cur.next.next != None:#如果最后只剩一个结点，或者cur移动到了最后一个结点，是不需要继续交换的
            temp1 = cur.next #记录真实头节点位置
            temp2 = cur.next.next.next#记录第三个结点位置

            cur.next = cur.next.next #第二个结点与第一个结点交换
            cur.next.next = temp1
            cur.next.next.next = temp2
            cur = cur.next.next #cur向后移动两位
        return dummyNode.next

```

#### 删除链表的倒数第 N 个节点

[19. 删除链表的倒数第 N 个节点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

![删除链表的倒数第 N 个节点通过记录](./assets/remove-nth-node-from-end-accepted.png)

```
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummyNode = ListNode(0 , head)
        slow = fast = dummyNode #快慢指针都从虚拟节点出发
        #fast先走n+1步，slow再出发
        for _ in range(n+1):
            fast = fast.next
        #slow出发，直至fast=NULL停止
        while fast is not None:
            slow = slow.next
            fast = fast.next
        #此时slow指向被删除节点的前一个
        slow.next = slow.next.next #删除了倒数第n个节点
        return dummyNode.next
```

#### 链表相交

[面试题 02.07. 链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)

![链表相交通过记录](./assets/intersection-of-two-linked-lists-accepted.png)

```python
#要让短链的尾部与长链的尾部对齐，因为两个链只要有交点，那么从交点开始，后面所有节点都是一样的，这是由于节点的next只有一个值决定的
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        curA , curB = headA , headB #两个指针都从两个链的头节点出发
        lenA , lenB = 0 , 0
        #算链A的长度
        while curA is not None:
            lenA += 1
            curA = curA.next
        #算链B的长度
        while curB is not None:
            lenB += 1
            curB = curB.next
        curA , curB = headA , headB
        #让A是长链，B是短链
        if lenA < lenB:
            curA , curB = headB , headA
            lenA , lenB = lenB , lenA
        #移动curA，与curB对齐
        for _ in range(lenA - lenB):
            curA = curA.next
        while curB is not None:
            if curA == curB:
                return curA
            else:
                curA = curA.next
                curB = curB.next
        return None
```

这题要利用节点的next只有一个值的特点。所以只需要让两个链的尾部对齐就可以了，长链指针向前移动（长链长度-短链长度）位。

#### 环形链表 II

[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

![环形链表 II 通过记录](./assets/linked-list-cycle-ii-accepted.png)

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow , fast = head , head#快指针和慢指针同时从头节点出发，快指针每次走两步，慢指针每次走一步
        while fast and fast.next:#如果有一个指向None，说明没有环
            fast = fast.next.next
            slow = slow.next
            #快慢指针在环中相遇了
            if slow == fast:
                slow = head #慢指针立刻从头节点出发，快指针从相遇点出发
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        return None
```

上面是双指针法，下面还有一个更简单的哈希集合法

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        visited = set()

        while head:#只有head不等于None，就说明有环
            if head in visited:#说明head这个节点被访问过
                return head
            visited.add(head)
            head = head.next
        return None
```
