### 题目：

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

### 解题思路：

从根结点开始，分别对两棵树进行中序遍历（根->左->右，使用递归），然后合并两个结点，作为结果树相应的结点。

```swift
class Solution {
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        guard t1 != nil || t2 != nil else {
            return nil
        }

        var result: TreeNode? = TreeNode.init(0)
        preListTrees(t1, t2, &result)
        return result
    }

    func preListTrees(_ t1: TreeNode?, _ t2: TreeNode?, _ result: inout TreeNode?) {
        var val = 0
        if let node1 = t1 {
            val = val + node1.val
        }
        if let node2 = t2 {
            val = val + node2.val
        }
        result?.val = val

        if t1?.left != nil || t2?.left != nil {
            var left: TreeNode? = TreeNode.init(0)
            result?.left = left
            preListTrees(t1?.left, t2?.left, &left)
        }

        if t1?.right != nil || t2?.right != nil {
            var right: TreeNode? = TreeNode.init(0)
            result?.right = right
            preListTrees(t1?.right, t2?.right, &right)
        }
    }
}
```

### 升级版：

以其中一棵树作为结果树，优化算法。

```swift
class Solution {
    func mergeTrees(_ t1: TreeNode?, _ t2: TreeNode?) -> TreeNode? {
        if t1 == nil || t2 == nil {
            return t1 == nil ? t2 : t1
        } else {
            t1?.val = (t1?.val)!+(t2?.val)!
            t1?.left = mergeTrees(t1?.left, t2?.left)
            t2?.right = mergeTrees(t1?.right, t2?.right)
            return t1
        }
    }
}
```
