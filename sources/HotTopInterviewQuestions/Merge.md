### 题目：

给定两个有序整数数组 nums1 和 nums2，将 nums2 合并到 nums1 中，使得 num1 成为一个有序数组。

说明:

> 初始化 nums1 和 nums2 的元素数量分别为 m 和 n。
> 你可以假设 nums1 有足够的空间（空间大小大于或等于 m + n）来保存 nums2 中的元素。
示例:

```
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6], n = 3

输出: [1,2,2,3,5,6]
```

### 解法一：暴力法

将nums2所有元素插入到nums1数据元素的后面，然后整个数组排序。

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        for i in 0..<nums2.count {
            nums1[i+nums1.count-nums2.count] = nums2[i]
        }
        nums1 = nums1.sorted()
    }
}
```

### 解法二：直接插入法

将nums1数据元素后面的空位移除，然后遍历nums2，一个一个元素插入nums1。

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        nums1.removeSubrange(m..<nums1.count)
        var index = 0
        for num in nums2 {
            while index < nums1.count, nums1[index] <= num {
                index += 1
            }
            nums1.insert(num, at: index)
        }
    }
}
```

### 解法三：头插入法

遍历nums1和nums2，将最小的数据元素插入nums1的第一位，不断的将nums1和nums2比较的数据元素插入前面，得到最终结果。

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        let array = nums1
        var i = 0
        var j = 0
        var k = 0
        while i < m, j < n {
            let tmp1 = array[i]
            let tmp2 = nums2[j]
            if tmp1 < tmp2 {
                nums1[k] = tmp1
                i += 1
            } else {
                nums1[k] = tmp2
                j += 1
            }
            k += 1
        }
        print("i: \(i) j: \(j) k: \(k)")
        if i < m {
            while i < m {
                nums1[k] = array[i]
                i += 1
                k += 1
            }
        } else if j < n {
            while j < n {
                nums1[k] = nums2[j]
                j += 1
                k += 1
            }
        }
    }
}
```

### 解法四：尾插入法

和解法三的思路一样，不过是从后面开始插入数据。

```swift
class Solution {
    func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
        if m == 0 {
            nums1 = nums2
            return
        }
        if n == 0 {
            return
        }
        var i = m-1
        var j = n-1
        for k in (0..<nums1.count).reversed() {
            let tmp1 = nums1[i]
            let tmp2 = nums2[j]
            if tmp1 > tmp2 {
                nums1[k] = tmp1
                i -= 1
            } else {
                nums1[k] = tmp2
                j -= 1
            }
            if i < 0 || j < 0 {
                break
            }
        }
        if j >= 0 {
            for k in 0...j {
                nums1[k] = nums2[k]
            }
        }
    }
}
```
