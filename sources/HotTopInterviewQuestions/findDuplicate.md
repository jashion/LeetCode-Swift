### 题目：

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

```
示例 1:

输入: [1,3,4,2,2]
输出: 2
示例 2:

输入: [3,1,3,4,2]
输出: 3
说明：
```

不能更改原数组（假设数组是只读的）。
只能使用额外的 O(1) 的空间。
时间复杂度小于 O(n2) 。
数组中只有一个重复的数字，但它可能不止重复出现一次。

### 解法一：

不考虑空间复杂度O(1)，可以使用Set。

```swift
 class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        var set = Set<Int>()
        for num in nums {
            if set.contains(num) {
                return num
            }
            set.insert(num)
        }
        return -1
    }
 }
```

### 解法二：

不考虑空间复杂度O(1)，还可以使用计数排序法。

```swift
 class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        var res = Array.init(repeating: 0, count: nums.count)
        for num in nums {
            if res[num] == 1 {
                return num
            }
            res[num] += 1
        }
        return -1
    }
 }
```

### 解法三：

考虑空间复杂度O(1)，时间复杂度小于O(n2)，使用排序，再对比的方法。（这里因为Swift数组传的值，而不是引用，其实空间复杂度为O(n)）

```swift
 class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        let res = nums.sorted()
        for i in 1..<res.count {
            if res[i] == res[i-1] {
                return res[i]
            }
        }
        return -1
    }
 }
```

### 解法四：

快慢指针法，空间复杂度O(1)，时间复杂度为O(n)。

可以使用快慢指针，是因为1～n个整数其中有一个重复的，那么遍历的时候必然会形成环，而交接点就是重复的数字。

```swift
 class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        var tortoise = nums[0]
        var hare = nums[0]
        repeat {
            tortoise = nums[tortoise]
            hare = nums[nums[hare]]
        } while(tortoise != hare)
        
        hare = nums[0]
        while tortoise != hare {
            tortoise = nums[tortoise]
            hare = nums[hare]
        }
        return hare
    }
 }
```
