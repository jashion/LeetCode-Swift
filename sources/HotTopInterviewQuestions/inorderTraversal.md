### 题目：

给定一个二叉树，返回它的中序 遍历。

```
示例:

输入: [1,null,2,3]
1
 \
  2
 /
3

输出: [1,3,2]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？
```

### 解法一：递归

就是一般的二叉树的中序遍历。

```swift
 class Solution {
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        guard root != nil else {
            return []
        }

        var res = [Int]()
        inorder(root, &res)
        return res
    }

    func inorder(_ root: TreeNode?, _ res: inout [Int]) {
        guard root != nil else {
            return
        }

        inorder(root?.left, &res)
        res.append(root!.val)
        inorder(root?.right, &res)
    }
 }
```

### 解法二：数组

使用迭代方式遍历，借助辅助数组。

```
比如：
    1
  /   \
 2     3
/  \
4   5
可以看作：
[1] -> [2 1 3] -> [425 1 3]
```

也就是，从根结点开始，然后在根结点前面插入左孩子，在后面插入右孩子，然后左右孩子也进行一样的操作，直到二叉树的所有结点都遍历完。

```swift
 class Solution {
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        guard root != nil else {
            return []
        }

        var res = [Int]()
        var nodes = [root]
        var indexes = [0]
        while indexes.count > 0 {
            let tmp = indexes
            let tmpNodes = nodes
            indexes.removeAll()
            for i in tmp {
                let node = tmpNodes[i]
                let start = indexes.count
                var left = false
                if node?.left != nil {
                    left = true
                    let j = start + i
                    nodes.insert(node?.left, at: j)
                    indexes.append(j)
                }
                if node?.right != nil {
                    var j = 0
                    if left {
                        j = start + i + 2
                    } else {
                        j = start + i + 1
                    }
                    nodes.insert(node?.right, at: j)
                    indexes.append(j)
                }
            }
        }
        res = nodes.map{$0!.val}
        return res
    }
 }
```

### 解法三：栈

使用迭代方式遍历，借助辅助栈结构。遍历二叉树的最左子结点，然后输出，再从最左子结点的右孩子，开始，遍历右孩子的最左子结点，然后输出，直到遍历完整个二叉树。

```swift
 //stack
 class Solution {
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        var res = [Int]()
        var stack = [TreeNode]()
        var cur = root
        while cur != nil || !stack.isEmpty {
            while cur != nil {
                stack.append(cur!)
                cur = cur?.left
            }
            cur = stack.removeLast()
            res.append(cur!.val)
            cur = cur?.right
        }
        return res
    }
 }
```

### 解法四：线索二叉树

将树状结构，以中序遍历的次序，遍历一条链表。每次都是把父结点作为父结点的左孩子的最右结点的右孩子，因为中序遍历，可定是先遍历完父结点的左子树，然后再到父结点，再到父结点的右子树，因为，父结点的左孩子的最右结点的下一个结点就是父结点，所以，直接把把父结点作为父结点的左孩子的最右结点的右孩子，是符合中序遍历的次序的。

```swift
 class Solution {
    func inorderTraversal(_ root: TreeNode?) -> [Int] {
        var res = [Int]()
        var cur = root
        var pre: TreeNode? = nil
        while cur != nil {
            if cur?.left == nil {
                res.append(cur!.val)
                cur = cur?.right
            } else {
                pre = cur?.left
                while pre?.right != nil {
                    pre = pre?.right
                }
                pre?.right = cur
                let tmp = cur
                cur = cur?.left
                tmp?.left = nil
            }
        }
        return res
    }
 }
```
