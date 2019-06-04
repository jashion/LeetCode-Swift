### 题目：

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

### 解题思路：

题目核心是构建平衡二叉树，那么可以使用二分法的思想，取数组中间的数为根结点，然后将数组分成根结点的左右子树，取左右子树的中间的数分别作为根结点的左右孩子，不断重复上面的操作，直到遍历数组的所有数据元素。（中序遍历的逆序）

```swift
class Solution {
    func sortedArrayToBST(_ nums: [Int]) -> TreeNode? {
        guard nums.count > 0 else {
            return nil
        }
        let rootIndex = nums.count/2
        let root: TreeNode? = TreeNode.init(nums[rootIndex])
        let i = rootIndex-1
        if i >= 0 {
            root?.left = sortedArrayToBST(Array(nums[0...i]))
        }
        let j = rootIndex+1
        if j < nums.count {
            root?.right = sortedArrayToBST(Array(nums[j..<nums.count]))
        }
        return root
    }
}
```

### 算法改进：

去掉下标判断。

```swift
class Solution {
    func sortedArrayToBST(_ nums: [Int]) -> TreeNode? {
        guard nums.count > 0 else {
            return nil
        }
        let mid = nums.count/2
        let root: TreeNode? = TreeNode.init(nums[mid])
        root?.left = sortedArrayToBST(Array(nums[0..<mid]))
        root?.right = sortedArrayToBST(Array(nums[(mid+1)..<nums.count]))
        return root
    }
}
```
