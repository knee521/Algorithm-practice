# Binary tree 学习心得

本目录用于记录二叉树专题的实现代码、遍历方法、解题思路、易错点和复盘结论。

## 学习内容

- [x] 二叉树基础与递归遍历
- [x] 二叉树的迭代遍历
- [x] 二叉树的统一迭代法
- [x] 层序遍历
- [x] 翻转二叉树
- [x] 对称二叉树
- [ ] 二叉树的属性
- [ ] 二叉搜索树
- [ ] 公共祖先与路径问题
- [ ] 二叉树综合复盘

## 核心思路

- 先明确节点结构、空节点处理方式和遍历顺序，再选择递归或迭代实现。
- 深度优先遍历关注左右子树的递归关系，广度优先遍历使用队列逐层处理。
- 涉及返回值、全局变量或路径状态时，先定义函数的输入、输出和不变量。

## 易错点与边界条件

> 二叉树定义
>
> ```python
> class TreeNode:
>     def __init__(self , val , left = None , right = None):
>         self.val = val
>         self.left = left
>         self.right = right
> ```
>
> #### 写递归函数三要素
>
> 1.确定递归函数的参数和返回值
>
> 2.确定终止条件
>
> 3.确定单层递归逻辑

## 复盘

> 每道题记录遍历方式、递归返回值、状态转移、边界处理、时间复杂度和空间复杂度。

## 代码记录

> 在这里补充每道二叉树题目的题目链接、实现代码和个人学习笔记。

#### 二叉树的前序遍历

[二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

![二叉树前序遍历学习截图](assets/preorder-traversal.png)

##### 递归遍历

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def digui(node):
            if node is None:
                return
            res.append(node.val)
            digui(node.left)
            digui(node.right)
        digui(root)
        return res
```

##### 迭代遍历

注意：比如前序遍历（中左右），要先把中节点放入栈，**再放右节点，再放左节点**。要以**出栈的顺序**为准。

```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if root is None:
            return []
        #定义栈
        stack = [root]#先把中节点放进去
        #定义结果列表
        result = []
        while stack:#栈不为空
            #处理栈顶的节点
            node = stack.pop()
            #先处理中节点
            result.append(node.val)
            #处理右节点---以出栈的顺序为准
            if node.right is not None:
                stack.append(node.right)
            #再处理左节点---以出栈的顺序为准
            if node.left is not None:
                stack.append(node.left)
        return result
```

##### 统一迭代法

要点就是在中结点后加一个空指针，另外要以出栈的顺序为准

```python
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if root is None:
            return []
        #定义栈
        stack = [root]#先把中节点放进去
        #定义结果列表
        result = []
        while stack:#栈不为空
            node = stack.pop()
            if node:
                if node.right:
                    stack.append(node.right)
                if node.left:
                    stack.append(node.left)
                stack.append(node)
                stack.append(None)
            else:
                node = stack.pop()
                result.append(node.val)
        return result
```



#### 二叉树后序遍历

[二叉树后续遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

![二叉树后序遍历学习截图](assets/postorder-traversal.png)

##### 递归遍历

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def digui(node):
            if node is None:
                return
            digui(node.left)
            digui(node.right)
            res.append(node.val)
        digui(root)
        return res
```

##### 迭代遍历

改前序遍历的逻辑即可：
后序遍历的逻辑是左右中。前序遍历的逻辑是中左右，所有中左右--》中右左--》反转result数组就是左右中

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if root is None:
            return []
        #定义栈
        stack = [root]
        #定义结果数组
        result = []
        while stack:#栈不为空
            #先处理栈顶结点
            node = stack.pop()
            #存入值
            result.append(node.val)
            #再处理左结点
            if node.left:
                stack.append(node.left)
            #最后处理右节点
            if node.right:
                stack.append(node.right)
        return result[::-1]#起始位置：结束位置：遍历方向  -1代表从后往前遍历，起始位置和结束位置为空代表按照默认的，所有result[::-1]代表翻转数组，顺序变成了左右中
```

##### 统一迭代法

```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if root is None:
            return []
        #定义栈
        stack = [root]
        #定义结果数组
        result = []
        while stack:#栈不为空
            node = stack.pop()
            if node:
                stack.append(node)
                stack.append(None)
                if node.right:
                    stack.append(node.right)
                if node.left:
                    stack.append(node.left)
            else:
                node = stack.pop()
                result.append(node.val)
        return result
```



#### 二叉树的中序遍历

[二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

![二叉树中序遍历学习截图](assets/inorder-traversal.png)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: TreeNode | None) -> list[int]:
        res = []
        def digui(node):
            if node is None:
                return
            digui(node.left)
            res.append(node.val)
            digui(node.right)
        digui(root)
        return res
```

##### 迭代遍历

注意：中序遍历的思路和前序遍历的思路不一样，他要用一个指针来辅助遍历

```python
class Solution:
    def inorderTraversal(self, root: TreeNode | None) -> list[int]:
        if root is None:
            return []
        #定义栈
        stack = []
        #定义结果列表
        result = []
        #定义指针，用来遍历节点，从根节点开始
        cur = root
        while cur or stack:#两个全空就结束了
            if cur:#指针指向的节点非空
                stack.append(cur)
                cur = cur.left
            else:#指针指向的节点为空
                cur = stack.pop()#让指针指向栈顶结点，并弹出栈顶结点  左
                result.append(cur.val)
                cur = cur.right                   #                右
        return result
```

##### 统一迭代法

```python
class Solution:
    def inorderTraversal(self, root: TreeNode | None) -> list[int]:
        if root is None:
            return []
        #定义栈
        stack = [root]
        #定义结果列表
        result = []
        while stack:
            node = stack.pop()
            if node:
                if node.right:
                    stack.append(node.right)
                stack.append(node)
                stack.append(None)
                if node.left:
                    stack.append(node.left)
            else:
                node = stack.pop()
                result.append(node.val)
        return result
```

#### 层序遍历

[二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

![二叉树层序遍历学习截图](assets/level-order-traversal.png)

```python
class Solution:
    def levelOrder(self, root: TreeNode | None) -> list[list[int]]:
        if not root:
            return []
        #定义一个双端的队列
        myque = deque([root])#[root] 是只含一个节点的列表
        #定义结果列表
        result = []
        while myque:
            level = []#保存每一层的遍历结果
            for _ in range(len(myque)):
                cur = myque.popleft()
                level.append(cur.val)
                if cur.left:
                    myque.append(cur.left)
                if cur.right:
                    myque.append(cur.right)
            result.append(level)
        return result
```

##### 递归法



```python
class Solution:
    def levelOrder(self, root: TreeNode | None) -> list[list[int]]:
        if not root:
            return []
        #存储每一层的结点值
        levels = []
        def mydigui(node , level):
            if not node:
                return []
            #结束条件
            if len(levels) == level:#在第一次到达某一层时，为这一层创建一个空列表
#第一次到达第 0 层：levels 是 []，长度为 0，于是加入一个空列表，变成 [[]]。
#第一次到达第 1 层：levels 是 [[根节点的值]]，长度为 1，于是再加入一个空列表。之后再次到达第 1 层：levels 已经有两层，长度为 2，无需重复创建。
                levels.append([])
            levels[level].append(node.val)
            mydigui(node.left , level + 1)
            mydigui(node.right , level + 1)
        mydigui(root , 0)
        return levels
```

#### 二叉树的层序遍历Ⅱ

![二叉树层序遍历 II 学习截图](assets/level-order-bottom.png)

[二叉树的层序遍历Ⅱ](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/description/)

```python
class Solution:
    def levelOrderBottom(self, root: TreeNode | None) -> list[list[int]]:
        if not root:
            return []
        #定义一个双端队列
        myque = deque([root])
        #定义结果列表
        result = []
        while myque:
            level = []#保存每一层的遍历结果
            for _ in range(len(myque)):
                cur = myque.popleft()
                level.append(cur.val)
                if cur.left:
                    myque.append(cur.left)
                if cur.right:
                    myque.append(cur.right)
            result.append(level)
        return result[::-1]
```

其实就是从上往下层序遍历的结果进行反转

##### 递归法

```python
class Solution:
    def levelOrderBottom(self, root: TreeNode | None) -> list[list[int]]:
        if not root:
            return []
        #存储每一层的结点值
        levels = []
        def digui(node , level):
            if not node:
                return []
            #到达某一层的时候，初始化一个空的列表
            if len(levels) == level:
                levels.append([])
            levels[level].append(node.val)
            digui(node.left , level + 1)
            digui(node.right , level + 1)
        digui(root , 0)
        return levels[::-1]
```

#### 二叉树的右视图

[二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/description/)

![二叉树右视图学习截图](assets/right-side-view.png)

```python
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        #先判断root是否为空
        if not root:
            return []
        #定义结果数组
        result = []
        #定义两端的队列
        myque = deque([root])
        while myque:
            level_size = len(myque)
            for i in range(level_size):
                cur = myque.popleft()
                if i == level_size - 1:#这三个if必须放在for里面，因为如果把这三个 if 移到 for 外面，就无法逐个检查当前层的节点，也无法逐个把它们的孩子加入队列。
                    result.append(cur.val)
                if cur.left:
                    myque.append(cur.left)
                if cur.right:
                    myque.append(cur.right)
        return result
```

#### 二叉树的层平均值

[二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/description/)

![二叉树层平均值学习截图](assets/average-of-levels.png)

```python
class Solution:
    def averageOfLevels(self, root: TreeNode | None) -> list[float]:
        if not root:
            return []
        #定义结果数组
        result = []
        #定义两端的队列
        myque = deque([root])
        while myque:
            level_size = len(myque)#当前层的长度大小，因为每次刚进入某一层的时候，上一层的元素都被pop掉了
            level_sum = 0#每一层的总和
            for i in range(level_size):
                node = myque.popleft()
                level_sum += node.val
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
            result.append(level_sum / level_size)
        return result
```

#### N叉树的层序遍历

[N叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/description/)

![N 叉树层序遍历学习截图](assets/n-ary-level-order.png)

```python
class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        #定义两端的队列
        myque = deque([root])
        #定义结果列表
        result = []
        while myque:
            level_size = len(myque)#获取每一层的长度
            level = []#保存每一层的遍历结果
            for i in range(level_size):
                node = myque.popleft()
                level.append(node.val)
                for child in node.children:
                    myque.append(child)
            result.append(level)
        return result

```

#### 在每个树行中找最大值

[在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)

![每层最大值学习截图](assets/largest-values.png)

```python
class Solution:
    def largestValues(self, root: TreeNode | None) -> list[int]:
        if not root:
            return []
        #定义结果列表
        result = []
        #定义两端队列
        myque = deque([root])

        while myque:
            #每一层的最大值，不能初始化为0，如果某一层的节点值全部是负数，最大值就会错误地得到 0.所以要把初始值改为负无穷大
            max_value = float('-inf')
            for i in range(len(myque)):
                node = myque.popleft()
                max_value = max(max_value , node.val)
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
            result.append(max_value)
        return result
```

float('-inf')代表负无穷大，float('inf')代表正无穷大

#### 填充每个节点的下一个右侧节点指针

[填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/description/)

![填充右侧节点指针学习截图](assets/connect-next-right.png)

```python

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root:
            return root
        #定义两端队列
        myque = deque([root])
        #前一个结点
        pre = None
        while myque:
            level_size = len(myque)
            #前一个结点
            pre = None#每一层开始的时候都要更新
            for i in range(level_size):
                node = myque.popleft()
                if pre:
                    pre.next = node
                pre = node
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return root
```

#### 填充每个节点的下一个右侧节点指针II

[填充每个节点的下一个右侧节点指针II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/description/)

![填充右侧节点指针 II 学习截图](assets/connect-next-right-ii.png)

```python
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return root
        #定义两端队列
        myque = deque([root])
        while myque:
            level_size = len(myque)
            pre = None
            for i in range(level_size):
                node = myque.popleft()
                if pre:
                    pre.next = node
                pre = node
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return root
```

#### 二叉树的最大深度

[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)

二叉树的深度就是根节点到最远叶子节点的最长路径上的节点数。

![二叉树最大深度学习截图](assets/maximum-depth.png)

```python
class Solution:
    def maxDepth(self, root: TreeNode | None) -> int:
        if not root:
            return 0
        myque = deque([root])
        deepth = 0
        while myque:
            deepth += 1
            level_size = len(myque)
            for i in range(level_size):
                node = myque.popleft()
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return deepth
```

#### 二叉树的最小深度

[二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/description/)

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

![二叉树最小深度学习截图](assets/minimum-depth.png)

```python
class Solution:
    def minDepth(self, root: TreeNode | None) -> int:
        #只有当左右孩子都为空的时候，才说明遍历的最低点了。如果其中一个孩子为空则不是最低点
        if not root:
            return 0
        myque = deque([root])
        deepth = 0
        while myque:
            deepth += 1
            level_size = len(myque)
            for i in range(level_size):
                node = myque.popleft()
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
                if not node.left and not node.right:
                    return deepth
        return deepth
```

#### 翻转二叉树

[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/)

只要保证每个结点的左右子节点交换就可以

![翻转二叉树学习截图](assets/invert-binary-tree.png)

##### 递归法

前序遍历

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        root.left , root.right = root.right , root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

##### 层序遍历

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        myque = deque([root])
        while myque:
            level_size = len(myque)
            for i in range(level_size):
                node = myque.popleft()
                node.left , node.right = node.right , node.left
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return root
```

##### 迭代法

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        stack = [root]
        while stack:
            node = stack.pop()
            node.left , node.right = node.right , node.left
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return root
```

#### 对称二叉树

[对称二叉树](https://leetcode.cn/problems/symmetric-tree/description/)

![对称二叉树学习截图](assets/symmetric-tree.png)

##### 递归法

```python
class Solution:
    def compare(self , left , right):
        #首先排除空结点情况
        if left is None and right is not None:
            return False
        elif left is not None and right is None:
            return False
        elif left is None and right is None:
            return True
        #再排除非空但数值不同的情况
        elif left.val != right.val:
            return False
        #开始递归
        outside = self.compare(left.left , right.right)#外
        inside = self.compare(left.right , right.left)#内
        result = outside and inside
        return result
    def isSymmetric(self, root: TreeNode | None) -> bool:
        if not root:
            return True
        result = self.compare(root.left , root.right)
        return result
```

##### 层序遍历法

```python
class Solution:
    def isSymmetric(self, root: TreeNode | None) -> bool:
        if not root:
            return True
        myque = deque([root])
        while myque:
            level_size = len(myque)
            level_val = []#存储每一层节点的值
            for i in range(level_size):
                node = myque.popleft()
                if node:
                    level_val.append(node.val)
                    myque.append(node.left)
                    myque.append(node.right)
                else:
                    level_val.append(None)
            if level_val != level_val[::-1]:
                return False
        return True
```

#### 相同的树

[相同的树](https://leetcode.cn/problems/same-tree/description/)

![相同的树学习截图](assets/same-tree.png)

##### 递归法

```python
class Solution:
    def isSameTree(self, p: TreeNode | None, q: TreeNode | None) -> bool:
        if p is not None and q is None:
            return False
        elif p is None and q is not None:
            return False
        elif p is None and q is None:
            return True
        elif p.val != q.val:
            return False
        #已经排除节点为空、不为空但值不同的情况，下面继续比较两棵树对应的子节点的情况
        leftsame = self.isSameTree(p.left , q.left)#比较两个树的左节点是否相同
        rightsame = self.isSameTree(p.right , q.right)#比较两个树的右节点是否相同
        return leftsame and rightsame
```

##### 层序遍历法

```python
class Solution:
    def isSameTree(self, p: TreeNode | None, q: TreeNode | None) -> bool:
        myque_p = deque([p])
        myque_q = deque([q])
        while myque_p and myque_q:
            #去除节点
            node_p = myque_p.popleft()
            node_q = myque_q.popleft()
            #比较节点
            if node_p is None and node_q is None:
                continue
            if node_p is None or node_q is None:
                return False
            if node_p.val != node_q.val:
                return False
            myque_p.append(node_p.left)
            myque_p.append(node_p.right)#即使节点是空的，也要把None放入队列中
            myque_q.append(node_q.left)
            myque_q.append(node_q.right)#即使节点是空的，也要把None放入队列中
        return not myque_p and not myque_q
```

#### 另一颗树的子树

[另一棵树的子树](https://leetcode.cn/problems/subtree-of-another-tree/)

![另一棵树的子树学习截图](assets/subtree-of-another-tree.png)

##### 递归法

```python
class Solution:
    def isSame(self , p , q):
        if p is None and q is None:
            return True
        if p is None or q is None:
            return False
        if p.val != q.val:
            return False
        leftsame = self.isSame(p.left , q.left)
        rightsame = self.isSame(p.right , q.right)
        return leftsame and rightsame

    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if subRoot is None:
            return True
        elif root is None:
            return False
        if self.isSame(root , subRoot):
            return True
        #往下找一下左右节点是否存在另一颗子树
        left = self.isSubtree(root.left , subRoot)
        right = self.isSubtree(root.right , subRoot)
        return left or right
```

##### 层序遍历法

```python
class Solution:
    #用来判断以改起点为根节点的两棵树是否相同
    def isSametree(self , p , q ):
        myque_p = deque([p])
        myque_q = deque([q])
        while myque_p and myque_q:
            #去除节点
            node_p = myque_p.popleft()
            node_q = myque_q.popleft()
            #比较节点
            if node_p is None and node_q is None:
                continue
            if node_p is None or node_q is None:
                return False
            if node_p.val != node_q.val:
                return False
            myque_p.append(node_p.left)
            myque_p.append(node_p.right)#即使节点是空的，也要把None放入队列中
            myque_q.append(node_q.left)
            myque_q.append(node_q.right)#即使节点是空的，也要把None放入队列中
        return not myque_p and not myque_q

    #用来找子树的对应匹配的起点
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        myque = deque([root])
        while myque:
            node = myque.popleft()
            if self.isSametree(node , subRoot):
                return True
            #说明起点找错了，继续往下找左右子树节点
            if node.left:
                myque.append(node.left)
            if node.right:
                myque.append(node.right)
        #走到这里了，说明没有找到子树
        return False
```

#### N叉树的最大深度

[N叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

![N叉树的最大深度学习截图](assets/n-ary-tree-maximum-depth.png)

```python
class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        myque = deque([root])
        maxdepth = 0
        while myque:
            maxdepth += 1
            level_size = len(myque)
            for i in range(level_size):
                node = myque.popleft()
                for child in node.children:
                    myque.append(child)
        return maxdepth
```



#### 完全二叉树的节点个数

[完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/description/)

![完全二叉树的节点个数学习截图](assets/count-complete-tree-nodes.png)

##### 二叉树的层序遍历方法

这个方法统计是正确的，但是效率不满足力扣的条件

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        myque = deque([root])
        result = 0
        while myque:
            level_size = len(myque)
            for i in range(level_size):
                result += 1#记录节点数量
                node = myque.popleft()
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return result
```

##### 递归法

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        #分别统计从根节点出发的最左路径和最右路径的高度
        node = root
        left_height = 0
        while node:
            left_height += 1
            node = node.left

        node = root
        right_height = 0
        while node:
            right_height += 1
            node = node.right
        #如果左右子树的最大高度相同，说明是满二叉树
        if left_height == right_height:
            return 2 ** left_height - 1
        else:
            return (self.countNodes(root.left) + self.countNodes(root.right) + 1)#加1是加上根节点，到这里的时候node已经是None了
```

#### 平衡二叉树

[平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/description/)

![平衡二叉树学习截图](assets/balanced-binary-tree.png)

##### 递归法

```python
class Solution:
    #用来获取改节点的高度
    #如果以该节点为根节点，他不是平衡二叉树，即左右子树高度差超过1，那么返回-1
    #如果是二叉树，即左右子树高度差小于等于1，那么返回该节点的高度值
    def getHeight(self , node):
        #确认终止条件
        if node is None:
            return 0
        #确认单层递归逻辑:后序遍历用来求高度
        leftHeight = self.getHeight(node.left)
        if leftHeight == -1:
            return -1
        rightHeight = self.getHeight(node.right)
        if rightHeight == -1:
            return -1
        if abs(leftHeight - rightHeight) > 1:#不是平衡二叉树
            return -1
        else:#左右子树高度差小于等于1，是平衡二叉树
            return (1 + max(leftHeight , rightHeight))
    def isBalanced(self, root: TreeNode | None) -> bool:
        if self.getHeight(root) == -1:
            return False
        else:
            return True
```

#### 二叉树的所有路径

[二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/description/)

![二叉树的所有路径学习截图](assets/binary-tree-paths.png)

```python
class Solution:
    #首先确认递归参数：当前节点、path数组记录路径值、result列表记录结果，字符串类型
    #终止条件是遇到叶节点，即当前节点不为空，但是左右子节点为空
    def mydigui(self , node , path , result):
        #单层递归遍历，前序遍历
        path.append(node.val)#中
        #终止条件
        if node.left is None and node.right is None:
            str_path = '->'.join(map(str , path))#map是迭代器，用来把path这个对象转换为str类型
            result.append(str_path)
            return result
        #单层递归遍历，前序遍历
        if node.left:#左
            self.mydigui(node.left , path , result)
            path.pop()#回溯
        if node.right:
            self.mydigui(node.right , path , result)
            path.pop()#回溯

    def binaryTreePaths(self, root: TreeNode | None) -> list[str]:
        path = []
        result = []
        if root is None:
            return result
        self.mydigui(root , path , result)
        return result
```

#### 左叶子之和

[左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/description/)

![左叶子之和学习截图](assets/sum-of-left-leaves.png)

```python
class Solution:
    #确定递归参数：根节点，返回左叶子之和
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        #终止条件：当前节点是None或者当前节点的左右子节点都是None
        if root is None:
            return 0
        if root.left is None and root.right is None:
            return 0
        #单层递归逻辑：记录左子树和右子树的左叶子数值，最后把这两个数相加.后序遍历
        left_val = self.sumOfLeftLeaves(root.left)
        if root.left is not None and root.left.left is None and root.left.right is None:#左叶子节点的父节点不为空，其左右子节点为空
            left_val = root.left.val
        right_val = self.sumOfLeftLeaves(root.right)#右子树右边的叶子节点的值不属于左叶子，所以不考虑
        mysum = left_val + right_val
        return mysum
```

#### 找树左下角的值

[找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/description/)

![找树左下角的值学习截图](assets/find-bottom-left-tree-value.png)

##### 层序遍历法

```python
class Solution:
    def findBottomLeftValue(self, root: TreeNode | None) -> int:
        if root is None:
            return 0
        myque = deque([root])
        result = 0#用来记录最后一行最左边的元素
        while myque:
            level_size = len(myque)
            for i in range(level_size):
                node = myque.popleft()
                if i == 0:
                    result = node.val
                if node.left:
                    myque.append(node.left)
                if node.right:
                    myque.append(node.right)
        return result
```

#### 路径总和

[路径总和](https://leetcode.cn/problems/path-sum/)

![路径总和学习截图](assets/path-sum.png)

##### 递归法

```python
class Solution:
    #确定递归参数：根节点、目标值
    def mydigui(self , node , target):
        #终止条件：找到叶子节点并且target等于0或者已经到叶子节点了，但是target还没等于0
        if node.left is None and node.right is None and target == 0:
            return True
        if node.left is None and node.right is None and target != 0:
            return False
        #单层递归逻辑：前序遍历，
        if node.left:
            if self.mydigui(node.left , target - node.left.val):
                return True#因为不需要遍历整个树，所以要有返回值。如果要遍历整个树，就不要返回值
        if node.right:
            if self.mydigui(node.right , target - node.right.val):
                return True

        return False
    def hasPathSum(self, root: TreeNode | None, targetSum: int) -> bool:
        if root is None:
            return False
        return self.mydigui(root , targetSum - root.val)
```

#### 路径总和Ⅱ

[路径总和Ⅱ](https://leetcode.cn/problems/path-sum-ii/description/)

![路径总和Ⅱ学习截图](assets/path-sum-ii.png)

##### 递归法

```python
class Solution:
    #定义实例属性
    def __init__(self):
        self.result = []#记录最终所有路径值
        self.path = []#记录路径值
    #确认递归参数：根节点、目标值
    def mydigui(self , node , target):
    #递归结束条件：遇到叶子节点并且target等于0或者遇到叶子节点target不等于0
        if node.left is None and node.right is None and target == 0:
            #这里存在的问题是：保存的是同一个路径列表的引用，没有保存当时路径的副本。 后续回溯执行 self.path.pop() 时，result 中已经保存的路径也会跟着变化。
            #所以用.copy()会创建一个新的列表，保存当前路径。后面修改 self.path，就不会影响这个副本
            self.result.append(self.path.copy())
            return
        if node.left is None and node.right is None:
            return
    #单层递归逻辑:前序遍历，递归前，把当前子节点的值放到path里
        if node.left:
            self.path.append(node.left.val)
            self.mydigui(node.left , target - node.left.val)
            self.path.pop()#把node.left弹出，回溯到根节点，从而遍历右子节点,退回当前父节点对应的路径
        if node.right:
            self.path.append(node.right.val)
            self.mydigui(node.right , target - node.right.val)
            self.path.pop()

    def pathSum(self, root: TreeNode | None, targetSum: int) -> list[list[int]]:
        if root is None:
            return self.result
        self.path.append(root.val)
        self.mydigui(root , targetSum - root.val)
        return self.result
```

#### 从中序与后序遍历序列构造二叉树

[从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

![从中序与后序遍历序列构造二叉树学习截图](assets/build-tree-inorder-postorder.png)

```python
class Solution:
    def buildTree(self, inorder: list[int], postorder: list[int]) -> TreeNode | None:
        #后序数组为空，说明没有根节点
        if not postorder:
            return None#这个返回值会被赋值给左右子节点，子节点的类型是TreeNode，而[]是列表类型，出现了类型冲突
        #在后序中找切割点，也就是中间节点
        root_val = postorder[-1]
        newroot = TreeNode(val = root_val)
        #在中序中根据切割点进行切割
        separator_id =  inorder.index(root_val)#直接返回中序数组中root_val的下标
        inorder_left = inorder[ : separator_id]#这是左闭右开区间
        inorder_right = inorder[separator_id + 1 : ]#这是左闭右开区间
        #根据中序中切割的左中序和右中序进行切割后序数组,因为后序数组的左右后序数组的长度和中序数组的左右中序数组长度一样
        postorder_left = postorder[ : len(inorder_left)]#这是左闭右开区间
        postorder_right = postorder[len(inorder_left) : len(postorder) - 1]#这是左闭右开区间,要去掉最后一个根节点.左右中
        #最后递归左右区间
        newroot.left = self.buildTree(inorder_left , postorder_left)
        newroot.right = self.buildTree(inorder_right , postorder_right)
        return newroot
```

#### 从前序与中序遍历序列构造二叉树

[从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

![从前序与中序遍历序列构造二叉树学习截图](assets/build-tree-preorder-inorder.png)

```python
class Solution:
    def buildTree(self, preorder: list[int], inorder: list[int]) -> TreeNode | None:
        #前序数组为空，说明没有根节点
        if not preorder:
            return None
        #在前序列表中找切割点，就是前序数组的第一个元素为根节点
        root_val = preorder[0]
        root = TreeNode(val = root_val)
        #在中序列表中找到切割点的下标，进行切割成左右中序列表
        separator_id = inorder.index(root_val)
        inorder_left = inorder[ : separator_id]#左闭右开
        inorder_right = inorder[separator_id + 1 : ]#要跳过中间节点
        #根据中序列表切割成的左右中序列表，进行切割前序列表，因为前序列表的左右前序列表的长度和中序列表的左右中序列表长度一样
        preorder_left = preorder[1 : 1 + len(inorder_left)]#这里截至下标也要加1，这样就相当于整个数组都往右移动一位，就可以跳过根节点
        preorder_right = preorder[1 + len(inorder_left) : ]
        #递归左右子树
        root.left = self.buildTree(preorder_left , inorder_left)
        root.right = self.buildTree(preorder_right , inorder_right)
        return root
```

#### 最大二叉树

[最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/description/)

![最大二叉树学习截图](assets/maximum-binary-tree.png)

```python
class Solution:
    #首先确认递归参数：左或右区间的数组
    def constructMaximumBinaryTree(self, nums: list[int]) -> TreeNode | None:
        #终止条件
        if len(nums) == 1:#传入的数组长度是1，说明到了叶子节点
            node = TreeNode(nums[0])
            return node
        #单层递归逻辑
        #1.先找到区间内的最大值及其下标，并创建以其为根节点的树
        max_value = max(nums)
        max_id = nums.index(max_value)
        node = TreeNode(max_value)
        #2.根据最大值（根节点）的下标切割左右子树区间
        #3.分别递归左右子树
        if max_id > 0:#左
            nums_left = nums[ : max_id]#左闭右开
            node.left = self.constructMaximumBinaryTree(nums_left)
        if max_id < len(nums) - 1:#右
            nums_right = nums[max_id + 1 : ]#左闭右开
            node.right = self.constructMaximumBinaryTree(nums_right)
        return node
```

#### 合并二叉树

[合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/description/)

![合并二叉树学习截图](assets/merge-two-binary-trees.png)

```python
class Solution:
    #确认递归参数：两个树的根节点，返回合并树的根节点,把root1作为合并树的根节点
    def mergeTrees(self, root1: TreeNode | None, root2: TreeNode | None) -> TreeNode | None:
        #递归结束条件：root1为None，返回root2节点，root2为None，返回root1节点
        if root1 is None :
            return root2
        if root2 is None:
            return root1
        #单层递归逻辑:前序遍历
        #把root1节点的值加上root2对应节点的值
        root1.val += root2.val #中
        #root1.left.val += root2.left.val#左 如果加这两句，那么root1的左右子节点就重复加了两次
        #root1.right.val += root2.right.val#右
        #左右子树分别进行递归
        root1.left = self.mergeTrees(root1.left , root2.left)#左
        root1.right = self.mergeTrees(root1.right , root2.right)#右
        return root1
```

#### 二叉搜索树中的搜索

[二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)

![二叉搜索树中的搜索学习截图](assets/search-in-a-binary-search-tree.png)

```python
#二叉搜索树是一个有序树：
#若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
#若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
#它的左、右子树也分别为二叉搜索树
class Solution:
    #确定递归的参数：根节点、目标值，返回目标节点
    def searchBST(self, root: TreeNode | None, val: int) -> TreeNode | None:
        #终止条件：1.根节点为None 2.找到目标值
        if root is None or root.val == val:
            return root
        #单层递归逻辑
        #1.如果根节点的值大于val，就递归左子树
        #2.如果根节点的值小于val，就递归右子树
        if root.val > val :
            result = self.searchBST(root.left , val)
        if root.val < val:
            result = self.searchBST(root.right , val)
        return result
```

#### 验证二叉搜索树

[验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/description/)

![验证二叉搜索树学习截图](assets/validate-binary-search-tree.png)

```python
class Solution:
    def __init__(self):
        self.maxvalue = float('-inf')
    #递归参数：根节点，返回布尔类型
    def isValidBST(self, root: TreeNode | None) -> bool:
        #结束条件：根节点为None
        if root is None:
            return True
        #单层递归逻辑：中序遍历
        #左
        result_left = self.isValidBST(root.left)
        #中
        if self.maxvalue < root.val:
            self.maxvalue = root.val
        else:
            return False
        #右
        result_right = self.isValidBST(root.right)
        return result_left and result_right

```

#### 二叉搜索树的最小绝对差

[二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/description/)

![二叉搜索树的最小绝对差学习截图](assets/minimum-absolute-difference-in-bst.png)

```python
class Solution:
    def __init__(self):
        self.pre = None
        self.result = float('inf')
    #确定递归参数：根节点，无返回值，因为有全局变量result
    def mydigui(self , node):
         #终止条件：当前根节点为None
        if node is None:
            return
        #单层递归逻辑：中序遍历
        self.mydigui(node.left)
        #保存node-pre的最小值
        if self.pre is not None:
            self.result = min(self.result , node.val - self.pre.val)
        self.pre = node
        self.mydigui(node.right)

    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.mydigui(root)
        return self.result
```

#### 二叉搜索树中的众数

[二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)

![二叉搜索树中的众数学习截图](assets/find-mode-in-binary-search-tree.png)

```python
class Solution:
    def __init__(self):
        self.pre = None
        self.count = 0
        self.maxcount = 0
        self.result =[]
    #确定递归参数：根节点
    def mydigui(self , node):
        #结束条件
        if node is None:
            return
        #单层递归逻辑：中序遍历
        #因为二叉搜索的特点：结点左子树中所含节点的值 小于等于 当前节点的值，结点右子树中所含节点的值 大于等于 当前节点的值
        #二叉搜索树中序遍历是非递减的数组。所以相同的值一定挨在一起
        self.mydigui(node.left)
        if self.pre is None:
            self.count = 1
        elif self.pre.val == node.val:
            self.count += 1
        elif self.pre.val != node.val:
            self.count = 1
        self.pre = node#更新前一个结点
        if self.count == self.maxcount:
            self.result.append(node.val)
        if self.count > self.maxcount:
            self.maxcount = self.count
            self.result = []
            self.result.append(node.val)
        self.mydigui(node.right)

    def findMode(self, root: TreeNode | None) -> list[int]:
        self.mydigui(root)
        return self.result
```

#### 二叉树的最近祖先

[二叉树的最近祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

![二叉树的最近公共祖先学习截图](assets/lowest-common-ancestor-of-a-binary-tree.png)

```python
class Solution:
    #递归参数：根节点，p和q
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        #结束条件：根节点为空，返回None；根节点等于p或q，返回根节点
        if root is None:
            return None
        if root == p or root == q:
            return root
        #单层递归逻辑：后序遍历
        left = self.lowestCommonAncestor(root.left , p , q)
        right = self.lowestCommonAncestor(root.right , p , q)
        if left is not None and right is not None:#left和right不是布尔类型，所以不能写成not left
            return root
        elif left is not None and right is None:
            return left
        elif left is None and right is not None:
            return right
        else:
            return None

```

1. 求最小公共祖先，需要从底向上遍历，那么二叉树，只能通过后序遍历（即：回溯）实现从底向上的遍历方式。
2. 在回溯的过程中，必然要遍历整棵二叉树，即使已经找到结果了，依然要把其他节点遍历完，因为要使用递归函数的返回值（也就是代码中的left和right）做逻辑判断。
3. 要理解如果返回值left为空，right不为空为什么要返回right，为什么可以用返回right传给上一层结果。

#### 二叉搜索树的最近公共祖先

[二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

![二叉搜索树的最近公共祖先学习截图](assets/lowest-common-ancestor-of-a-binary-search-tree.png)

从上往下搜索，遇到第一个结点的值在p和q之间，那么这点结点就是p和q的最近公共祖先

```python

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        #递归结束条件：根节点为None
        if root is None:
            return root
        #单层递归逻辑：后序遍历  只要当前节点值在p和q之间，就直接返回
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left , p , q)
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right , p ,q)
        return root

```

#### 二叉搜索树中的插入操作

[二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/description/)

![二叉搜索树中的插入操作学习截图](assets/insert-into-a-binary-search-tree.png)

```python
class Solution:
    #找到空节点插进去
    def insertIntoBST(self, root: TreeNode | None, val: int) -> TreeNode | None:
        if root is None:
            node = TreeNode(val)
            return node
        #单层递归逻辑
        if root.val > val:
            root.left = self.insertIntoBST(root.left , val)
        if root.val < val:
            root.right = self.insertIntoBST(root.right , val)
        return root
```

#### 删除二叉搜索树中的节点

[删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

![删除二叉搜索树中的节点学习截图](assets/delete-node-in-a-bst.png)

```python
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        #结束条件：根节点为None
        if root is None:
            return root
        #单层递归逻辑
        #找打了当前节点的值等于key
        if root.val == key:
            if root.left is None and root.right is None:
                return None
            elif root.left is None:
                return root.right
            elif root.right is None:
                return root.left
            elif root.left is not None and root.right is not None:
                cur = root.right
                #找到右子树的最左边的节点，将左子树放到右子树最左边节点上，并返回右子树的根节点
                while cur.left is not None:
                    cur = cur.left
                cur.left = root.left
                return root.right
        #左右子树递归
        if root.val > key:
            root.left = self.deleteNode(root.left , key)
        if root.val < key:
            root.right = self.deleteNode(root.right , key)
        return root
```

#### 修建二叉搜索树

[修建二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/description/)

![修剪二叉搜索树学习截图](assets/trim-a-binary-search-tree.png)

```python
class Solution:
    def trimBST(self, root: TreeNode | None, low: int, high: int) -> TreeNode | None:
        if root is None:
            return root
        #单层递归逻辑
        #先寻找有效节点
        if root.val < low:
            #从根节点的右子树寻找符合条件的节点，并返回其根节点
            return self.trimBST(root.right , low , high)
        if root.val > high:
            return self.trimBST(root.left , low , high)
        #将寻找到的有效节点返回给根节点
        root.left = self.trimBST(root.left , low , high)
        root.right = self.trimBST(root.right , low , high)
        return root
```

#### 将有序数组转换为二叉搜索树

[将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)

![将有序数组转换为二叉搜索树学习截图](assets/convert-sorted-array-to-binary-search-tree.png)

```python
class Solution:
    def mydigui(self ,nums , left , right):
        #如果左边界元素大于右边界元素，就返回None.左闭右闭区间
        if left > right:return None
        #单层递归逻辑
        mid = (left + right)  // 2
        node = TreeNode(nums[mid])
        node.left = self.mydigui(nums , left , mid - 1)
        node.right = self.mydigui(nums , mid + 1 , right)
        return node

    def sortedArrayToBST(self, nums: list[int]) -> TreeNode | None:
        root = self.mydigui(nums , 0 ,len(nums) - 1)
        return root
```
