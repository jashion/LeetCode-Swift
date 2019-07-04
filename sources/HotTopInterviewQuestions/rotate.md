### 题目：

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。

```
示例 1:

输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
示例 2:

输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释:
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
要求使用空间复杂度为 O(1) 的 原地 算法。

### 解法一：

暴力法，直接将后面的元素移除，然后再插入最前面的位置。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }
        for _ in 1...count {
            nums.insert(nums.removeLast(), at: 0)
        }
    }
 }
```

### 解法二：

暴力法，不过需要借助另外一个数组。先把nums[nums.count-k...nums.count-1]的数据加入数组，然后再把nums[0...(nums.count-1-k)]添加进数组，就能得出结果。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }
        var res = [Int]()
        for i in 1...count {
            res.insert(nums[nums.count-i], at: 0)
        }
        res.append(contentsOf: nums[0...(nums.count-1-count)])
        nums = res
    }
 }
```

### 解法三：

暴力法，先插入，再移除。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }
        nums.insert(contentsOf: nums[nums.count-count...nums.count-1], at: 0)
        nums.removeSubrange(Range((nums.count-count)...(nums.count-1)))
    }
 }
```

### 解法四：

以k个数为一组，如果数据个数不是k的倍数，则在nums.count-k处补足0。然后，不断的将nums[nums.count-k...nums.count-1]的数据和nums[i...i+2]（i<=nums.count%k）互换，然后移除最后的n个0。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }

        var i = count
        var max = nums.count - count
        var zeros = 0
        if nums.count%count != 0 {
            zeros = count - nums.count%count
        }
        nums.insert(contentsOf: Array.init(repeating: 0, count: zeros), at: max)
        max = nums.count - count
        while i <= max {
            for j in 0..<count {
                nums.swapAt(i-count+j, max+j)
            }
            i += count
        }
        if zeros > 0 {
            nums.removeSubrange(Range((nums.count-zeros)...(nums.count-1)))
        }
    }
 }
```

### 解法五：

需要借助额外的数组，直接找到每个数字得出结果后的下标，然后直接赋值。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }

        let tmp = nums
        let len = tmp.count
        for i in 0..<len {
            nums[i] = tmp[(len-count+i)%len]
        }
    }
 }
```

### 解法六：

和解法五的思想一样，解法五是找从下标0开始赋值，解法六则从下标(k%nums.count+i)%nums.count开始。

```swift
 class Solution {
    func rotate(_ nums: inout [Int], _ k: Int) {
        guard nums.count > 0 else {
            return
        }
        let count = k%nums.count
        guard count > 0 else {
            return
        }

        let tmp = nums
        let len = tmp.count
        for i in 0..<len {
            nums[(count+i)%len] = tmp[i]
        }
    }
 }
```
