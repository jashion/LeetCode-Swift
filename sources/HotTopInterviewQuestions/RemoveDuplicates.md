### 题目：

给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

示例 1:

```
给定数组 nums = [1,1,2],

函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。

你不需要考虑数组中超出新长度后面的元素。
```

示例 2:

```
给定 nums = [0,0,1,1,1,2,2,3,3,4],

函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。

你不需要考虑数组中超出新长度后面的元素。
```

### 解法一：暴力法

遍历数组，遇到和前一个数字相等的就直接移除。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 1 else {
            return nums.count
        }

        var i = 1
        while i < nums.count {
            if nums[i] == nums[i-1] {
                nums.remove(at: i)
            } else {
                i += 1
            }
        }
        return nums.count
    }
}
```

和暴力法实现的思路一样，只不过不是一个个删除重复的元素，而是找到所有重复的元素一起移除。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var index = nums.count-1
        while index > 0 {
            let value = nums[index]
            let endIndex = index
            while index > 0 && nums[index-1] == value {
                index = index-1
            }
            if endIndex > index {
                nums.removeSubrange(Range.init(NSMakeRange(index+1, endIndex-index))!)
            } else {
                index = index-1
            }
        }
        return nums.count
    }
}
```

### 解法二：快慢指针

使用快慢指针，慢指针指向下一个不相等的数字的下标，快指针则是遍历数组的指针。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 1 else {
            return nums.count
        }

        var i = 0
        var j = 1
        while j < nums.count {
            if nums[j] == nums[i] {
                j += 1
            } else {
                j = j+1
                i += 1
                nums[i] = nums[j]
            }
        }
        return i+1
    }
}
```
