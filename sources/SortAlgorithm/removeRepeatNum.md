### 题目：

给定一个排序数组，你需要在**原地**删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。

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

### 更高效的解法：

使用奇偶双下标法，一次遍历就可以得到结果数据。
