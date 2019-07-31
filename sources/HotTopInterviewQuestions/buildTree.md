### 题目：

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

```
例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

3
/ \
9  20
/  \
15   7
```

### 解法一：

前序遍历，递归法。

```swift
 //递归
 class Solution {
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        guard preorder.count > 0, inorder.count > 0 else {
            return nil
        }
        let root = TreeNode.init(preorder[0])
        let rootIndex = inorder.firstIndex(of: root.val)!
        if rootIndex > 0 {
            let pre = Array(preorder[1...rootIndex])
            let inn = Array(inorder[0...(rootIndex-1)])
            root.left = buildTree(pre, inn)
        }
        if rootIndex < inorder.count-1 {
            let pre = Array(preorder[(rootIndex+1)...(preorder.count-1)])
            let inn = Array(inorder[(rootIndex+1)...(inorder.count-1)])
            root.right = buildTree(pre, inn)
        }
        return root
    }
 }
```

### 解法二：

前序遍历，递归法优化，将查找元素下标的操作，替换成，根据元素从字典取下标的操作，效率大大提高。

```swift
 //递归优化
 class Solution {
    var preIndex = 0
    var preorder = [Int]()
    var inorder = [Int]()
    var dic = [Int:Int]()

    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        guard preorder.count > 0, inorder.count > 0 else {
            return nil
        }

        self.preorder = preorder
        self.inorder = inorder

        for i in 0..<inorder.count {
            dic[inorder[i]] = i
        }

        return helper(0, inorder.count)
    }

    func helper(_ left: Int, _ right: Int) -> TreeNode? {
        if left == right {
            return nil
        }
        let rootVal = preorder[preIndex]
        let root = TreeNode.init(rootVal)

        let index = dic[rootVal]!

        preIndex += 1

        root.left = helper(left, index)
        root.right = helper(index+1, right)

        return root
    }
 }
```

### 解法三：

迭代法，层遍历。

```swift
 //迭代，层遍历
 class Solution {
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        guard preorder.count > 0, inorder.count > 0 else {
            return nil
        }
        var preNodes = preorder
        var inNodes = inorder
        let root = TreeNode.init(preNodes[0])
        var stack = [TreeNode]()
        stack.append(root)
        while !stack.isEmpty {
            let node = stack.removeFirst()
            let preIndex = preNodes.firstIndex(of: node.val)!
            let inIndex = inNodes.firstIndex(of: node.val)!
            if inIndex-preIndex>0 {
                node.left = TreeNode.init(preNodes[preIndex+1])
                stack.append(node.left!)
            }
            if inIndex+1 < preNodes.count, (stack.first == nil || stack.first!.val != preNodes[inIndex+1]) {
                node.right = TreeNode.init(preNodes[inIndex+1])
                stack.append(node.right!)
            }
            inNodes.remove(at: inIndex)
            preNodes.remove(at: preIndex)
        }
        return root
    }
}
```

### 解法四：

迭代法，层遍历，优化。不移除已添加元素，将元素的下标用字典存起来，提高了效率。

```swift
 //迭代优化
 class Solution {
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        guard preorder.count > 0, inorder.count > 0 else {
            return nil
        }
        let root = TreeNode.init(preorder.first!)
        var stack = [([Int], TreeNode)]()
        stack.append(([0, preorder.count-1], root))
        
        var preDic = [Int:Int]()
        for i in 0..<preorder.count {
            preDic[preorder[i]] = i
        }
        var inDic = [Int:Int]()
        for i in 0..<inorder.count {
            inDic[inorder[i]] = i
        }

        while !stack.isEmpty {
            let item = stack.removeFirst()

            let node = item.1
            let parentRange = item.0
            
            let preIndex = preDic[node.val]!
            let inIndex = inDic[node.val]!
            
            if inIndex > parentRange[0], preIndex + 1 < preorder.count {
                node.left = TreeNode.init(preorder[preIndex+1])
                stack.append(([parentRange[0], inIndex-1], node.left!))
            }
            if inIndex < parentRange[1], preIndex+inIndex-parentRange[0]+1 < preorder.count {
                node.right = TreeNode.init(preorder[preIndex+inIndex-parentRange[0]+1])
                stack.append(([inIndex+1, parentRange[1]], node.right!))
            }
        }
        return root
    }
 }
```
