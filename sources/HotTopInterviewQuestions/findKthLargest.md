### 题目：

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

```
示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。
```

### 解法一：

先排序，再取值。

```swift
 class Solution {
    func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
        return nums.sorted()[nums.count-k]
    }
 }
```

### 解法二：

冒泡排序法，假如k>=nums.count/2，从最小开始排，直到nums.count-k；假如k<nums.count/2，从最大开始排，直到nums=k+1。

```swift
 class Solution {
    func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
        var res = nums
        if k >= res.count/2 {
            for i in 0..<res.count-1 {
                for j in 0..<res.count-1-i {
                    if res[j] < res[j+1] {
                        res.swapAt(j, j+1)
                    }
                }
                if i == res.count - k {
                    break
                }
            }
            return res[k-1]
        } else {
            for i in 0..<res.count-1 {
                for j in 0..<res.count-1-i {
                    if res[j] > res[j+1] {
                        res.swapAt(j, j+1)
                    }
                }
                if i + 1 == k {
                    break
                }
            }
            return res[res.count-k]
        }
    }
 }
```

### 解法三：

堆排序法，构建长度为k的小顶堆，当堆的长度为k+1时，移除最小的元素也就是根结点，继续构建小顶堆，直到遍历完整个数组，那么整个小顶堆中的元素就是前k个最大的元素，根结点的元素就是第k个最大元素。（下面的代码并不是使用堆结构来实现的，只是思想一样，效率比真正的堆结构要低）

```swift
 class Solution {
    var heap = [Int]()

    func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
        for num in nums {
            add(num)
            print(heap)
            if heap.count > k {
                poll()
            }
        }
        return poll()
    }

    func add(_ num: Int) {
        if heap.isEmpty {
            heap.append(num)
        } else {
            var i = 0
            while i < heap.count, num < heap[i] {
                i += 1
            }
            heap.insert(num, at: i)
        }
    }

    func poll() -> Int {
        return heap.removeLast()
    }
 }
```

### 解法四：

使用二分法，逐渐逼近真正的解。

```swift
 class Solution {
    func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
        return divide(nums, k)
    }

    func divide(_ nums: [Int], _ k: Int) -> Int {
        let priot = nums[0]
        var left = [Int]()
        var right = [Int]()
        for i in 1..<nums.count {
            if nums[i] < priot {
                left.append(nums[i])
            } else {
                right.append(nums[i])
            }
        }
        if k == right.count+1 {
            return priot
        }
        if k <= right.count {
            return divide(right, k)
        } else {
            return divide(left, k-right.count-1)
        }
    }
 }
```

### 解法五：

快速排序，也有二分法的思想在里面，获取前nums.count-k个最小的元素就可以得到第k个最大的元素。

```swift
 //快速排序
 class Solution {
    var nums = [Int]()

    func findKthLargest(_ nums: [Int], _ k: Int) -> Int {
        self.nums = nums
        return quickSelect(0, nums.count-1, nums.count-k)
    }

    func quickSelect(_ left: Int, _ right: Int, _ kSmallest: Int) -> Int {
        if left == right {
            return nums[left]
        }

        var pivotIndex = left + Int(arc4random())%(right-left)
        pivotIndex = partition(left, right, pivotIndex)

        if kSmallest == pivotIndex {
            return nums[kSmallest]
        } else if kSmallest < pivotIndex {
            return quickSelect(left, pivotIndex-1, kSmallest)
        } else {
            return quickSelect(pivotIndex+1, right, kSmallest)
        }
    }

    func partition(_ left: Int, _ right: Int, _ pivotIndex: Int) -> Int {
        let pivot = nums[pivotIndex]
        nums.swapAt(pivotIndex, right)

        var storeIndex = left

        for i in left..<right {
            if nums[i] < pivot {
                nums.swapAt(storeIndex, i)
                storeIndex += 1
            }
        }

        nums.swapAt(storeIndex, right)

        return storeIndex
    }
 }
```
