### 题目：

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

```
示例 1:

输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:

输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。
```

### 解法一：

非原地算法，使用辅助数组。

```swift
 class Solution {
    func oddEvenList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil || head?.next?.next == nil {
            return head
        }

        var res = [ListNode]()
        var headNode = head
        var i = 0
        while headNode != nil {
            if i%2 == 0 {
                res.insert(headNode!, at: i/2)
            } else {
                res.append(headNode!)
            }
            headNode = headNode?.next
            i += 1
        }
        for i in 0..<res.count-1 {
            let node = res[i]
            node.next = res[i+1]
        }
        res.last!.next = nil
        return res.first!
    }
 }
```

### 解法二：

原地算法，使用奇偶链表。

```swift
 class Solution {
    func oddEvenList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil || head?.next?.next == nil {
            return head
        }
        var odd = head
        var even = head?.next
        let evenHead = even
        while even != nil, even?.next != nil {
            odd?.next = even?.next
            odd = odd?.next
            even?.next = odd?.next
            even = even?.next
        }
        odd?.next = evenHead
        return head
    }
 }
```

### 解法三：

原地算法，指针。

```swift
 class Solution {
    func oddEvenList(_ head: ListNode?) -> ListNode? {
        if head == nil || head?.next == nil || head?.next?.next == nil {
            return head
        }

        var p1 = head
        var p2 = head?.next
        while p2?.next != nil {
            let cur = p2?.next
            let tmp = cur?.next
            let next = p1?.next
            p1?.next = cur
            cur?.next = next
            p2?.next = tmp
            p1 = p1?.next
            p2 = p2?.next
        }
        return head
    }
 }
```
