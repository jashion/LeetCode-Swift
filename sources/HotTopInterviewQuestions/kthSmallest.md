### 题目：

给定一个二叉搜索树，编写一个函数 kthSmallest 来查找其中第 k 个最小的元素。

说明：
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

```
示例 1:

输入: root = [3,1,4,null,2], k = 1
     3
    / \
   1   4
    \
    2
   输出: 1
   示例 2:

   输入: root = [5,3,6,2,4,null,null,1], k = 3
      5
    /   \
   3     6
  / \
 2  4
/
1
   输出: 3
```

进阶：如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 kthSmallest 函数？   

###### 注意：二叉搜索树，是一棵特殊的二叉树。若它的左子树不为空，则左子树的所有结点都小于根结点；若它的右子树不为空，则右子树的所有结点都大于根结点。（不会有相同的数据元素）因此使用中序遍历，会得到有序的列表。

### 解法一：递归

使用递归实现中序遍历。

```swift
 class Solution {
    func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
        var res = [Int]()
        inorder(root, &res, k)
        return res[k-1]
    }

    func inorder(_ root: TreeNode?, _ res: inout [Int], _ k: Int) {
        guard root != nil else {
            return
        }
        inorder(root?.left, &res, k)
        res.append(root!.val)
        if res.count == k{
            return
        }
        inorder(root?.right, &res, k)
    }
 }
```

### 解法二：栈

使用栈实现中序遍历。

```swift
 class Solution {
    func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
        var res = [Int]()
        var stack = [TreeNode]()
        var cur: TreeNode? = root
        while cur != nil || !stack.isEmpty {
            if cur == nil {
                let last = stack.removeLast()
                res.append(last.val)
                cur = last.right
            } else {
                stack.append(cur!)
                cur = cur?.left
            }
        }
        return res.sorted()[k]
    }
 }
```

### 解法三：二分查找

先获取左子树的结点的数量，然后再和k比较，如果等于k-1，那么表示根结点就是第k个元素，如果大于k-1，则表示第k个元素在左子树，如果小于k-1，则表示第k个元素在右子树，继续遍历直到找到第k个元素。

```swift
 class Solution {
    func kthSmallest(_ root: TreeNode?, _ k: Int) -> Int {
        guard root != nil else {
            return 0
        }
        let leftCount = treeNodeCount(root?.left)
        if leftCount == k - 1 {
            return root!.val
        }
        if leftCount > k - 1 {
            return kthSmallest(root?.left, k)
        }
        return kthSmallest(root?.right, k - leftCount - 1)
    }
    
    func treeNodeCount(_ root: TreeNode?) -> Int {
        guard root != nil else {
            return 0
        }
        return 1 + treeNodeCount(root?.left) + treeNodeCount(root?.right)
    }
 }
```

进阶：如果频繁插入和删除元素，怎么优化？

可以使用二分法，不过需要记录左子树的结点数，插入和删除元素时，需要做相应的变化。如果第k个元素现在右子树，就可以省去遍历左子树的工作，提高效率。
