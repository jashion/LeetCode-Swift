### 题目：

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

      1    
     / \
     2  2
    / \ / \
    3 4 4 3

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

     1
    / \
    2 2
     \ \
      3 3

说明:

如果你可以运用递归和迭代两种方法解决这个问题，会很加分。

### 解法一：递归

从根结点开始，进行层遍历，并且判定每一次是否堆成，如果不是则改二叉树不是镜像对称的，直到遍历完，则改二叉树是镜像对称的。

```swift
class Solution {
    static var number = 0
    func isSymmetric(_ root: TreeNode?) -> Bool {
        return traversal([root])
    }

    func traversal(_ nodes: [TreeNode?]) -> Bool {
        var res: [TreeNode?] = Array.init(repeating: nil, count: nodes.count*2)
        for i in 0..<nodes.count {
            res[i*2] = nodes[i]?.left
            res[i*2+1] = nodes[i]?.right
        }

        var i = 0
        var j = res.count-1
        print(res)
        while i < j {
            let left = res[i]
            let right = res[j]

            if left != nil, right != nil {
                if left?.val != right?.val {
                    return false
                }
            }
            if (left != nil && right == nil) || (left == nil && right != nil) {
                return false
            }

            i += 1
            j -= 1
        }

        let tmp = res.filter { (node) -> Bool in
            node != nil
        }
        if tmp.isEmpty {
            return true
        } else {
            return traversal(tmp)
        }
    }
}
```

也是递归实现，因为如果过两个树互为镜像，则两颗树的根结点相等，并且左树的左孩子和右树的右孩子的值相等，左树的右孩子和右树的左孩子的值相等。

```swift
class Solution {
    func isSymmetric(_ root: TreeNode?) -> Bool {
        return isMirror(root?.left, root?.right)
    }

    func isMirror(_ left: TreeNode?, _ right: TreeNode?) -> Bool {
        if left == nil && right == nil {
            return true
        }
        if left == nil || right == nil {
            return false
        }

        return (left?.val == right?.val) && isMirror(left?.left, right?.right) && isMirror(left?.right, right?.left)
    }
}
```

### 解法二：迭代

实现的思路也是层遍历，不过使用的是迭代实现。

```swift
class Solution {
    static var number = 0
    func isSymmetric(_ root: TreeNode?) -> Bool {
        guard root != nil else {
            return true
        }
        var result: [TreeNode?] = [root]
        while !result.isEmpty {
            var res: [TreeNode?] = Array.init(repeating: nil, count: result.count*2)
            for i in 0..<result.count {
                res[i*2] = result[i]?.left
                res[i*2+1] = result[i]?.right
            }

            var i = 0
            var j = res.count-1
            while i < j {
                let left = res[i]
                let right = res[j]

                if left != nil, right != nil {
                    if left?.val != right?.val {
                        return false
                    }
                }
                if (left != nil && right == nil) || (left == nil && right != nil) {
                    return false
                }

                i += 1
                j -= 1
            }

            let tmp = res.filter { (node) -> Bool in
                node != nil
            }

            if tmp.isEmpty {
                return true
            }
            result = tmp
        }
        return true
    }
}
```

镜像的迭代实现。

```swift
class Solution {
    func isSymmetric(_ root: TreeNode?) -> Bool {
        var queue: [TreeNode?] = [root?.left, root?.right]
        while !queue.isEmpty {
            let left: TreeNode? = queue.first as? TreeNode
            queue = Array<TreeNode?>(queue.dropFirst(1))
            let right: TreeNode? = queue.first as? TreeNode
            queue = Array<TreeNode?>(queue.dropFirst(1))
            if left == nil && right == nil {
                continue
            }
            if left == nil || right == nil {
                return false
            }
            if left?.val != right?.val {
                return false
            }
            queue.append(left?.left)
            queue.append(right?.right)
            queue.append(left?.right)
            queue.append(right?.left)
        }
        return true
    }
}
```
