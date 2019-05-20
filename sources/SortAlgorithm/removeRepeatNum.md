### 题目：

给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**原地**修改输入数组并在使用 O(1) 额外空间的条件下完成。

⚠️注意：只要数组前面的数据元素没有重复的就可以。

### 解题思路：

下标index从0开始遍历，如果下标index的值和下标index-1的值相等，则删除index的值，直到index的值和index-1的值不想等，则开始下一个循环。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var index = 0
        while index < nums.count {
            let value = nums[index]
            if index > 0, value == nums[index-1] {
                while index < nums.count {
                    if nums[index] == value {
                        nums.remove(at: index)
                    } else {
                        break
                    }
                }
            }
            index = index + 1
        }
        return nums.count
    }
}
```

### 方法改进1：

重复数值不用一个个删除，而是找到最小下标和最大下标，一次性删除。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var index = 0
        while index < nums.count {
            let value = nums[index]
            var endIndex = index;
            while endIndex+1 < nums.count && nums[endIndex+1] == value {
                endIndex = endIndex+1
            }
            if endIndex > index {
                nums.removeSubrange(Range.init(NSMakeRange(index+1, endIndex-index))!)
            }
            index = index+1
        }
        return nums.count
    }
}
```

### 方法改进2:

前面下标都是从0开始的，这次下标换成从count-1开始。

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

### 更的解法：

前面的解题思路都是找到重复的数字删除掉，但是，其实可以只替换，不删除，只要保证前面的数据元素不重复就可以了。

```
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var length = nums.count
        var i = 0
        var j = 1
        var index = 1
        while i < nums.count && j < nums.count {
            if nums[i] != nums[j] {
                nums[index]=nums[j]
                index = index+1
                j = j+1
                i = j-1
            } else {
                j = j+1
                length = length-1
            }
        }
        return length
    }
}
```

### 更巧妙的解法进阶版：

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        var size = 0
        for i in 1..<nums.count {
            if nums[size] != nums[i] {
                size=size+1
                nums[size] = nums[i]
            }
        }
        return size+1
    }
}
```


