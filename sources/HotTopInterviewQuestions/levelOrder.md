### 题目：

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

```
例如:
给定二叉树: [3,9,20,null,null,15,7],

3
/ \
9  20
/  \
15   7
返回其层次遍历结果：

[
[3],
[9,20],
[15,7]
]
```

### 解法一：

递归，DFS深度遍历。

```swift
 class Solution {
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        guard root != nil else {
            return []
        }
        var res = [[Int]]()
        dfs(root!, 0, &res)
        return res
    }
    
    func dfs(_ root: TreeNode, _ level: Int, _ res: inout [[Int]]) {
        if res.count == level {
            res.append([])
        }
        
        var tmp = res[level]
        tmp.append(root.val)
        res[level] = tmp
        
        if root.left != nil {
            dfs(root.left!, level+1, &res)
        }
        if root.right != nil {
            dfs(root.right!, level+1, &res)
        }
    }
 }
```

### 解法二：

迭代，层序遍历，使用栈。

```swift
 class Solution {
    func levelOrder(_ root: TreeNode?) -> [[Int]] {
        guard root != nil else {
            return []
        }
        var stack = [TreeNode?]()
        stack.append(root)
        var res = [[Int]]()
        while !stack.isEmpty {
            var tmp = [Int]()
            for _ in 0..<stack.count {
                let node = stack.removeFirst()
                tmp.append(node!.val)
                if node?.left != nil {
                    stack.append(node?.left)
                }
                if node?.right != nil {
                    stack.append(node?.right)
                }
            }
            res.append(tmp)
        }
        return res
    }
 }
```
