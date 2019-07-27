### 题目：

在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序。

```
示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

### 解法一：

不考虑时间复杂度和空间复杂度的情况下，可以使用暴力插入法。

```swift
 //插入法
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        let headNode: ListNode = ListNode.init(0)
        var node = head
        while node != nil {
            let tmp = node?.next
            var pre: ListNode? = nil
            var cur = headNode.next
            while cur != nil, cur!.val < node!.val {
                pre = cur
                cur = cur?.next
            }
            if pre == nil {
                headNode.next = node
                node?.next = cur
            } else {
                pre?.next = node
                node?.next = cur
            }
            node = tmp
        }
        return headNode.next
    }
 }
```

### 解法二：

数组和系统排序。

```swift
//数组法一
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        guard head != nil else {
            return nil
        }
        var nodes = [ListNode]()
        var node = head
        while node != nil {
            nodes.append(node!)
            node = node?.next
        }
        nodes.sort { (n1, n2) -> Bool in
            n1.val < n2.val
        }
        for i in 0..<nodes.count-1 {
            let cur = nodes[i]
            cur.next = nodes[i+1]
        }
        let last = nodes.last
        last?.next = nil
        return nodes.first!
    }
 }

//数组法二
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        var root: ListNode? = head
        var array: [Int] = []
        while let node = root {
            array.append(node.val)
            root = node.next
        }
        guard array.count > 0 else {
            return nil
        }
        array.sort()
        var node: ListNode? = ListNode(array[0])
        let result = node
        for index in 1..<array.count {
            let temp = ListNode(array[index])
            node?.next = temp
            node = node?.next
        }
        return result
    }
 }
```

### 解法三：

归并排序，递归。时间复杂度为O(nlogn)，空间复杂度为O(n)。

```swift
 //归并排序：递归
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        return mergeSort(head)
    }

    func mergeSort(_ node: ListNode?) -> ListNode? {
        if node == nil || node?.next == nil {
            return node
        }
        var fast: ListNode? = node
        var slow: ListNode? = node
        var breadNode: ListNode? = node //需要将链表断开，在有序重连
        while fast != nil, fast?.next != nil {
            breadNode = slow
            slow = slow?.next
            fast = fast?.next?.next
        }
        breadNode?.next = nil
        let l1 = mergeSort(node)
        let l2 = mergeSort(slow)
        return merge(l1, l2)
   }

    func merge(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        if l1 == nil {
            return l2
        }
        if l2 == nil {
            return l1
        }
        if l1!.val < l2!.val {
            l1?.next = merge(l1?.next, l2)
            return l1
        } else {
            l2?.next = merge(l2?.next, l1)
            return l2
        }
    }
 }
```

### 解法四：

归并排序，迭代。时间复杂度为O(nlogn)，但是使用了辅助数组，所以空间复杂度为O(n)。

```swift
 //归并排序：迭代
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil {
            return head
        }
        var res = [ListNode]()
        var headNode = head
        while headNode != nil {
            let tmp = headNode?.next
            headNode?.next = nil
            res.append(headNode!)
            headNode = tmp
        }
        let length = res.count
        var i = 1
        while i < length {
            var j = 0
            while j < res.count {
                var l1 = res[j]
                if j + 1 < res.count {
                    l1 = merge(l1, res[j+1])!
                    res.remove(at: j+1)
                }
                res.remove(at: j)
                res.insert(l1, at: j)
                j += 1
            }
            i *= 2
        }
        return res.first
    }
    
    func merge(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        if l1 == nil {
            return l2
        }
        if l2 == nil {
            return l1
        }
        if l1!.val < l2!.val {
            l1?.next = merge(l1?.next, l2)
            return l1
        } else {
            l2?.next = merge(l2?.next, l1)
            return l2
        }
    }
 }
```

### 解法五：

归并排序，迭代。时间复杂度为O(nlogn)，因为在元素组上操作，所以，空间复杂度为O(1)。

```swift
 //归并排序：迭代
 class Solution {
    func sortList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil {
            return head
        }
        var length = 0
        var headNode = head
        while headNode != nil {
            length += 1
            headNode = headNode?.next
        }

        headNode = ListNode.init(-1)
        headNode?.next = head
        var i = 1
        var k = 1
        while i < length {
            //两个merge链表的间距
            let interval = Int(pow(Double(2), Double(k)))
            //从第一个结点开始
            var last: ListNode? = headNode
            //当树的深度为i时，分成几组merge
            for j in 0...length/interval {
                let pre = last?.next
                last?.next = nil
                let index = j*interval //每组下标开始的index
                var count = index
                var tmp: ListNode? = nil
                var cur: ListNode? = pre
                while cur != nil, count != index + interval/2 {
                    tmp = cur
                    cur = cur?.next
                    count += 1
                }
                //断开需要merge的第一个链表
                if tmp != nil {
                    tmp?.next = nil
                }
                tmp = cur
                while tmp != nil, count != index + interval - 1 {
                    tmp = tmp?.next
                    count += 1
                }
                let next = tmp?.next
                //断开需要merge的第二个链表
                if tmp != nil {
                    tmp?.next = nil
                }
                //merge
                let l1 = merge(pre, cur)
                //找到l1的最后一个结点
                var item = l1
                while item?.next != nil {
                    item = item?.next
                }
                //将l1和后面还没有merge的链表相连
                item?.next = next
                //将l1和之前已经merge的链表相连
                last?.next = l1
                //last为已经merge的最后一个结点
                last = item
            }
            i *= 2
            k += 1
        }
        return headNode?.next
    }

    func merge(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        if l1 == nil {
            return l2
        }
        if l2 == nil {
            return l1
        }
        if l1!.val < l2!.val {
            l1?.next = merge(l1?.next, l2)
            return l1
        } else {
            l2?.next = merge(l2?.next, l1)
            return l2
        }
    }
 }
```
