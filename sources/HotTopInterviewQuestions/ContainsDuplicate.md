### 题目：

给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

示例 1:

```
输入: [1,2,3,1]
输出: true
```
示例 2:

```
输入: [1,2,3,4]
输出: false
```
示例 3:

```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```

### 解法一：

使用Set检测是否有重复数字。

```swift
//检测长度
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count > 1 else {
            return false
        }
        var res = Set<Int>()
        for i in 0..<nums.count {
            res.insert(nums[i])
            if res.count < i+1 {
                return true
            }
        }
        return false
    }
}
//检测包含
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count > 1 else {
            return false
        }
        var res = Set<Int>()
        for value in nums {
            res.insert(value)
            if res.contains(value) {
                return true
            }
        }
        return false
    }
}
//简化
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        let res = Set(nums)
        if res.count < nums.count {
            return true
        } else {
            return false
        }
    }
}
//极简
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        return Set(nums).count != nums.count
    }
}
```

### 解法二：

使用字典，检测重复的数字。

```swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count > 1 else {
            return false
        }
        var res: [Int: Int] = [:]
        for value in nums {
            if res[value] != nil {
                return true
            } else {
                res[value] = value
            }
        }
        return false
    }
}
```

### 解法三：

数组先排序，然后相邻的元素检测是否有相等的。

```swift
class Solution {
    func containsDuplicate(_ nums: [Int]) -> Bool {
        guard nums.count > 1 else {
            return false
        }
        let res = nums.sorted()
        for i in 0..<res.count-1 {
            if res[i] == res[i+1] {
                return true
            }
        }
        return false
    }
}
```
