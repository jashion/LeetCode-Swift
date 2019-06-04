### 题目：

反转一个单链表。

**进阶:**  
使用迭代或递归地反转链表。

### 解题思路：

迭代：遍历链表，将所有结点存入一个数组，然后首尾两两交换数组的值。

```swift
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        var list: [ListNode?] = []
        var node = head
        while node != nil {
            list.append(node)
            node = node?.next
        }
        for i in 0..<list.count/2 {
            let leftNode = list[i]
            let rightNode = list[list.count-1-i]
            let temp = leftNode!.val
            leftNode?.val = rightNode!.val
            rightNode?.val = temp
        }
        return head
    }
}
```

### 常规迭代解法：

使用生成链表的头插法。

```swift
//头插法
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        var newHead: ListNode? = nil
        var p = head
        while p != nil {
            let tmp = p?.next
            p?.next = newHead
            newHead = p
            p = tmp
        }
        return newHead
    }
}
```

### 递归解法：

遍历到最后的结点，然后将最后结点的next指向前面的结点，前面的结点的next赋值为nil，不断递归，直到头结点。

```swift
//递归
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil {
            return head
        }

        var lastNode: ListNode? = head
        var preNode: ListNode? = lastNode
        while lastNode?.next != nil {
            preNode = lastNode
            lastNode = lastNode?.next
        }
        if lastNode! !== preNode! {
            lastNode?.next = preNode
            preNode?.next = nil
            reverseList(head)
        }
        return lastNode
    }
}
```

### 更简洁、高效的递归：

省去每次从head结点开始遍历的步骤，效率更高。

```swift
class Solution {
    func reverseList(_ head: ListNode?) -> ListNode? {
        guard let h = head, let next = h.next else {
            return head
        }
        
        let p = reverseList(next)
        next.next = head
        h.next = nil
        return p
    }
}
```


