### 题目：

给定一个大小为  *n* 的数组，找到其中的众数。众数是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

### 解题思路：

遍历数组，将每个数出现的次数统计出来，然后再找出出现次数最大的数。

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var majorityNumber = nums[0]
        var result: [Int:Int] = [:]
        for value in nums {
            if result.count == 0 || !result.keys.contains(value){
                result[value] = 1
            } else {
                result[value] = result[value]!+1
            }
        }
        if !result.isEmpty {
            majorityNumber = (result.sorted {(value1, value2) -> Bool in
                return value1.value>value2.value
            }.first?.key)!
        }

        return majorityNumber
    }
}
```

### 算法改进：

在获取众数的字典里，不用排序，使用遍历的方式。

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var majorityNumber = nums[0]
        var result: [Int:Int] = [:]
        for value in nums {
            if result.count == 0 || !result.keys.contains(value){
                result[value] = 1
            } else {
                result[value] = result[value]!+1
            }
        }
        if !result.isEmpty {
            var tmp:[Int:Int] = [:]
            for item in result {
                if tmp.isEmpty {
                    tmp[item.key] = item.value
                } else if item.value > (tmp.values.first)! {
                    tmp.removeAll()
                    tmp[item.key] = item.value
                }
            }
            majorityNumber = (tmp.keys.first)!
        }
        return majorityNumber
    }
}
```

### 另外的解法：

先排序，然后统计出现最多的数字。

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        let result = nums.sorted {$0<$1}
        var maxNumber = result[0]
        var maxCount = 0
        var number = maxNumber
        var count = maxCount
        for value in result {
            if value == number {
                count = count + 1
            } else {
                if count > maxCount {
                    maxNumber = number
                    maxCount = count
                }
                number = value
                count = 1
            }
        }
        if count > maxCount {
            maxNumber = number
        }
        return maxNumber
    }
}
```

### 最*666*的解法来了，摩尔投票法：

一个数组如果存在众数（出现次数大于n/2），有且仅有一个。摩尔投票法的关键在于，每一次删除两个不相等的数，最后剩下的肯定是众数。

理解：将数组分成两个，一个众数数组，一个其他数数组，然后每次都删除一个众数，一个其他数，则最后剩下的肯定是众数，因为众数的长度比其他数长。

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var majorityNum = nums[0]
        var count = 1
        for i in 1..<nums.count {
            if nums[i] == majorityNum {
                count = count + 1
            } else {
                count = count - 1
                if count == 0 {
                    majorityNum = nums[i+1]
                }
            }
        }
        return majorityNum
    }
}
```


