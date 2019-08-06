### 题目：

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

```
说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

3
/ \
9  20
/  \
15   7
返回它的最大深度 3 。
```

### 解法一：

DFS（深度优先遍历）也就是前序遍历。

```swift
 //DFS
 class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        if root == nil {
            return 0
        } else {
            var leftHeight = maxDepth(root?.left)
            var rightHeight = maxDepth(root?.right)
            return max(leftHeight, rightHeight) + 1
        }
    }
 }
 
//前序遍历
 class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        var maxDepth = 0
        preOrder(root, 0, &maxDepth)
        return maxDepth
    }

    func preOrder(_ root: TreeNode?, _ level: Int, _ maxLevel: inout Int) {
        if root == nil {
            return
        }
        maxLevel = max(maxLevel, level+1)
        preOrder(root?.left, level+1, &maxLevel)
        //maxLevel = max(maxLevel, level+1) 中序遍历
        preOrder(root?.right, level+1, &maxLevel)
        //maxLevel = max(maxLevel, level+1) 后序遍历
    }
 }
```

### 解法二：

BFS（广度优先遍历），也就是层遍历。

```swift
  //BFS
  class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        guard let tNode = root else {
            return 0
        }
        var depth = 0
        var tempNodes = [(TreeNode?, Int)]()
        tempNodes.append((tNode, 1))
        while !tempNodes.isEmpty {
            let node = tempNodes.removeFirst()
            let currentDepth = node.1
            if node.0 != nil {
                depth = max(depth, currentDepth)
                tempNodes.append((node.0?.left, currentDepth+1))
                tempNodes.append((node.0?.right, currentDepth+1))
            }
        }
        return depth
    }
 }

//层遍历1
 class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        guard let tNode = root else {
            return 0
        }
        var depth = 0
        var tempNodes: [TreeNode] = [tNode]
        while !tempNodes.isEmpty {
            depth = depth + 1
            let count = tempNodes.count
            for i in 0..<count {
                let node = tempNodes[i]
                if let lNode = node.left {
                    tempNodes.append(lNode)
                }
                if let rNode = node.right {
                    tempNodes.append(rNode)
                }
            }
            tempNodes.removeSubrange(Range.init(NSRange(location: 0, length: count))!)
        }
        return depth
    }
 }

//层遍历2
 class Solution {
    func maxDepth(_ root: TreeNode?) -> Int {
        guard let tNode = root else {
            return 0
        }
        var depth = 0
        var tempNodes = [(TreeNode, Int)]()
        tempNodes.append((tNode, 1))
        while !tempNodes.isEmpty {
            let node = tempNodes.removeFirst()
            let currentDepth = node.1
            depth = max(depth, currentDepth)
            if node.0.left != nil {
                tempNodes.append((node.0.left!, currentDepth+1))
            }
            if node.0.right != nil {
                tempNodes.append((node.0.right!, currentDepth+1))
            }
        }
        return depth
    }
 }
```
