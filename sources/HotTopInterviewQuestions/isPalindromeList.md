### 题目：

请判断一个链表是否为回文链表。

```
示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true
```

进阶：

你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

### 解法一：

数组法，遍历链表，将每个结点存进数组，然后再判断是否是回文链表。

```swift
 class Solution {
    func isPalindrome(_ head: ListNode?) -> Bool {
        guard head != nil else {
            return true
        }

        var res = [ListNode]()
        var node = head
        while node != nil {
            res.append(node!)
            node = node?.next
        }

        var i = 0
        var j = res.count-1
        while i < j {
            if res[i].val != res[j].val {
                return false
            }
            i += 1
            j -= 1
        }
        return true
    }
 }
```

### 解法二：

双指针+前半部分翻转（比较巧妙的是在遍历链表的时候同时翻转前半部分链表）。

```swift
 class Solution {
    func isPalindrome(_ head: ListNode?) -> Bool {
        if (head == nil || head?.next == nil) {
            return true
        }

        var slow = head
        var fast = head

        var reverse:ListNode? = nil

        while (fast?.next != nil) {
            fast = fast?.next?.next
            let next = slow?.next
            slow?.next = reverse
            reverse = slow
            slow = next
        }

        if fast != nil {
            slow = slow?.next
        }

        while slow != nil {
            if slow?.val != reverse?.val {
                return false
            }

            slow = slow?.next
            reverse = reverse?.next
        }
        return true
    }
 }
```

### 解法三：

双指针+后半部分翻转。

```swift
 class Solution {
    func isPalindrome(_ head: ListNode?) -> Bool {
        if head == nil || head?.next == nil {
            return true
        }
        if head?.next?.next == nil {
            return head!.val == head?.next!.val
        }
        
        var s = head
        var f = head
        while f != nil, f?.next != nil {
            s = s?.next
            f = f?.next?.next
        }
        f = head
        s?.next = reverse(s?.next)
        s = s?.next
        while s != nil {
            if f!.val != s!.val {
                return false
            }
            f = f?.next
            s = s?.next
        }
        return true
    }
    
    func reverse(_ head: ListNode?) -> ListNode? {
        var pre: ListNode? = nil
        var cur: ListNode? = head
        while cur != nil {
            let tmp = cur?.next
            cur?.next = pre
            pre = cur
            cur = tmp
        }
        return pre
    }
 }
```
