### 题目：

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

**示例：**

**输入：**1->2->4, 1->3->4
**输出：**1->1->2->3->4->4

### 解题思路：

思路很简单，就是将一条链表的结点逐一合并到另一条链表。

```swift
//迭代
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        guard l1 != nil, l2 != nil else {
            return l1 != nil ? l1 : l2
        }
        let head = (l1?.val)! <= (l2?.val)! ? l1 : l2
        var list2 = (l1?.val)! > (l2?.val)! ? l1 : l2
        while list2 != nil {
            let val2 = (list2?.val)!
            var list1 = head
            var temp: ListNode?
            while list1 != nil {
                let val1 = (list1?.val)!
                if val2 >= val1 {
                    temp = list1
                    if list1?.next == nil {
                        let newNode = ListNode(val2)
                        let tempNext = temp?.next
                        temp?.next = newNode
                        newNode.next = tempNext
                        break
                    }
                } else {
                    let newNode = ListNode(val2)
                    let tempNext = temp?.next
                    temp?.next = newNode
                    newNode.next = tempNext
                    break
                }
                list1 = list1?.next
            }
            list2 = list2?.next
        }
        return head
    }
}

```

### 算法改进：

去掉内部循环，使用双指针。

```swift
//迭代
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        guard l1 != nil, l2 != nil else {
            return l1 != nil ? l1 : l2
        }
        var list1 = l1
        var list2 = l2
        var head: ListNode? = nil
        var tail: ListNode? = nil
        while list1 != nil, list2 != nil {
            var tmp: ListNode? = nil
            if (list1?.val)! < (list2?.val)! {
                tmp = list1
                list1 = list1?.next
            } else {
                tmp = list2
                list2 = list2?.next
            }
            if head == nil {
                head = tmp
                tail = tmp
            } else {
                tail?.next = tmp
                tail = tmp
            }
        }
        if list1 != nil {
            tail?.next = list1
        }
        if list2 != nil {
            tail?.next = list2
        }
        return head
    }
}
```

### 更简洁的实现：

实现思路一样，但是代码更简洁。

```swift
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        var (p1,p2) = (l1,l2)
        var head = ListNode(0)
        var p = head

        while p1 != nil && p2 != nil {
            if p1!.val < p2!.val {
                p.next = p1
                p1 = p1!.next
            } else {
                p.next = p2
                p2 = p2!.next
            }
            p = p.next!
        }
        if p1 != nil {
            p.next = p1
        }
        if p2 != nil {
            p.next = p2
        }

        return head.next

    }
}
```

### 更简洁的实现2：

```swift
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        guard l1 != nil, l2 != nil else {
            return l1 != nil ? l1 : l2
        }
        let list1 = (l1?.val)! <= (l2?.val)! ? l1 : l2
        var list2 = (l1?.val)! > (l2?.val)! ? l1 : l2
        var index: ListNode? = list1
        while index != nil, list2 != nil {
            if let next = index?.next {
                if next.val > (list2?.val)! {
                    let tmp = list2?.next
                    list2?.next = next
                    index?.next = list2
                    list2 = tmp
                } else {
                    index = index?.next
                }
            } else {
                index?.next = list2
                break
            }
        }
        return list1
    }
}
```

### 让人惊叹的递归实现：

前面的几个算法实现都是使用迭代的方式，现在使用递归的方式，代码非常简洁。

```swift
class Solution {
    func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
        guard let l1 = l1 else{
            return l2
        }
        guard let l2 = l2 else{
            return l1
        }
        var result : ListNode? = (l1.val < l2.val) ? l1 : l2
        if l1.val < l2.val {
            result?.next = mergeTwoLists(l1.next, l2)
        }else{
            result?.next = mergeTwoLists(l1, l2.next)
        }

        return result
    }
}
```


