### 题目：

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组  `char[]`  的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

### 解题思路：

使用首位双指针，不断交换双指针之间的值，直到所有元素都已经交换完。

```swift
class Solution {
    func reverseString(_ s: inout [Character]) {
        var i = 0
        var j = s.count-1
        while i < j {
            s.swapAt(i, j)
            i = i+1
            j = j-1
        }
    }
}
```

### 算法改进：

当元素相同时，可以省去交换的步骤。

```swift
class Solution {
    func reverseString(_ s: inout [Character]) {
        var i = 0
        var j = s.count-1
        while i < j {
            if s[i] != s[j] {
                s.swapAt(i, j)
            }
            i = i+1
            j = j-1
        }
    }
}
```

### 最简洁的实现：

```swift
class Solution {
    func reverseString(_ s: inout [Character]) {
        s.reverse()
    }
}
```
