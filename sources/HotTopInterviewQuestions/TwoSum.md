### 题目：

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

### 解法一：暴力法

遍历所有组合，找出适合的解。

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        for i in 0..<nums.count {
            let item = nums[i]
            for j in i+1..<nums.count {
                let item2 = nums[j]
                if item+item2 == target {
                    return [i, j]
                }
            }
        }
        return []
    }
}
```

### 解法二：使用字典

因为每个数字只有一个，所以将所有的数字和下标存在字典里，数字为key，数字的下标为value，然后遍历字典，找出适合的组合。

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var numsDict: [Int:Int] = [:]
        for (index, item) in nums.enumerated() {
            numsDict[item] = index
        }
        for index in 0..<nums.count {
            let result = target - nums[index]
            if numsDict.keys.contains(result), numsDict[result] != index {
                return [index, numsDict[result]!]
            }
        }
        return []
    }
}
```

### 解法三：使用字典

和解法二的思路一样，算法的实现稍微调整一下，只需要使用一次遍历。

```swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var numsDict: [Int:Int] = [:]
        for (index, item) in nums.enumerated() {
            let value = target - item
            if numsDict.keys.contains(value) {
                return [numsDict[value]!, index]
            } else {
                numsDict[item] = index
            }
        }
        return []
    }
}
```
