### 题目：

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科]([https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin](https://baike.baidu.com/item/%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

![binarytree](images/binarytree.png)

```
示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
 输出: 3
 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
 示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
 输出: 5
 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

说明:

所有节点的值都是唯一的。
 p、q 为不同节点且均存在于给定的二叉树中。
```

### 解法一：

递归法，深度遍历二叉树，判断当前结点是否符合结点q，p，再分别判断左子树和右子树是否符合q，p，只要其中当前结点，左子树和右子树两个符合条件，说明当前结点就是LCA。

```swift
 class Solution {
    var res: TreeNode?

    func lowestCommonAncestor(_ root: TreeNode, _ p: TreeNode, _ q: TreeNode) -> TreeNode? {
        recurseTree(root, p, q)
        return res
    }

    func recurseTree(_ root: TreeNode?, _ p: TreeNode, _ q: TreeNode) -> Bool {
        if root == nil {
            return false
        }

        let mid = root?.val == p.val || root?.val == q.val ? 1 : 0
        let left = recurseTree(root?.left, p, q) ? 1 : 0
        let right = recurseTree(root?.right, p, q) ? 1 : 0

        if mid + left + right >= 2 {
            res = root
        }

        return (mid + left + right > 0)
    }
 }
```

### 解法二：

父指针迭代法，以子结点为key，父结点为value的数据结构形式，保存从根结点到q结点的路径上所有结点。然后，从q结点开始，把q结点的所有祖先都保存再一个数组里。最后，判断p结点以及p结点的所有祖先结点是否在该数组上，返回最近的一个公共祖先结点。

```swift
 class Solution {
    func lowestCommonAncestor(_ root: TreeNode, _ p: TreeNode, _ q: TreeNode) -> TreeNode? {
        var stack = [TreeNode]()
        var parent = [TreeNode:TreeNode?]()

        stack.append(root)
        parent[root] = nil

        while !parent.keys.contains(p) || !parent.keys.contains(q) {
            let node = stack.removeFirst()

            if node.left != nil {
                parent[node.left!] = node
                stack.append(node.left!)
            }
            if node.right != nil {
                parent[node.right!] = node
                stack.append(node.right!)
            }
        }

        var ancestors = Set<TreeNode>()

        var pNode: TreeNode? = p
        while pNode != nil {
            ancestors.insert(pNode!)
            pNode = parent[pNode!]!
        }

        var qNode: TreeNode? = q
        while !ancestors.contains(qNode!) {
            qNode = parent[qNode!]!
        }

        return qNode!
    }
 }
```

### 解法三：

无父指针迭代法，通过设置三种状态，已遍历当前结点，已遍历左孩子，已遍历右孩子，来表示一个结点被遍历的情况。当找到q或者p，则栈上的所有结点都是q或者p的祖先，那么，当再遍历到第二个符合条件的结点，最近的祖先就是LCA。

```swift
 class Solution {
    static var both_pending = 2
    static var left_done = 1
    static var both_done = 0
    func lowestCommonAncestor(_ root: TreeNode, _ p: TreeNode, _ q: TreeNode) -> TreeNode? {
        var stack = [[TreeNode:Int]]()
        stack.append([root : Solution.both_pending])
        var one_node_found = false
        var LCA: TreeNode? = nil
        var child_node: TreeNode? = nil
        
        while !stack.isEmpty {
            let top = stack.last!
            let parent_node = top.keys.first!
            let parent_state = top.values.first!
            
            if parent_state != Solution.both_done {
                if parent_state == Solution.both_pending {
                    if parent_node.val == p.val || parent_node.val == q.val {
                        if one_node_found {
                            return LCA
                        } else {
                            one_node_found = true
                            
                            LCA = parent_node
                        }
                    }
                    
                    child_node = parent_node.left
                } else {
                    child_node = parent_node.right
                }
                
                stack.removeLast()
                stack.append([parent_node:parent_state-1])
                
                if child_node != nil {
                    stack.append([child_node!: Solution.both_pending])
                }
            } else {
                let last = stack.removeLast().keys.first!
                if one_node_found, LCA?.val == last.val {
                    LCA = stack.last?.keys.first!
                }
            }
        }
    }
 }
```
