### 题目：

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

```
示例 1:

输入: [2,2,1]
输出: 1
示例 2:

输入: [4,1,2,1,2]
输出: 4
```

### 解法一：

字典法，统计每一个数字出现的次数，筛选出次数只出现一次的数字。

```swift
 class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        var map = [Int:Int]()
        for num in nums {
            if map[num] != nil {
                map[num] = map[num]! + 1
            } else {
                map[num] = 1
            }
        }
        return map.filter { (key, value) -> Bool in
            value == 1
        }.keys.first!
    }
 }
```

### 解法二：

集合法，假如集合没有包含该元素，则添加，反之，则删除集合中的该元素，返回集合中的唯一一个元素。

```swift
 class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        var map = Set<Int>()
        for num in nums {
            if map.contains(num) {
                map.remove(num)
            } else {
                map.insert(num)
            }
        }
        return map.first!
    }
 }
```

### 解法三：

数组法，将数组排序，然后寻找只出现一次的元素。

```swift
 class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        let orderNums = nums.sorted { $0 <= $1 }
        let length = orderNums.count
        for i in 0..<length {
            if i == 0 {
                if orderNums[0] != orderNums[1] {
                    return orderNums[0]
                }
            } else if i == length-1 {
                if orderNums[length-1] != orderNums[length-2] {
                    return orderNums[length-1]
                }
            } else {
                if orderNums[i-1] != orderNums[i], orderNums[i+1] != orderNums[i] {
                    return orderNums[i]
                }
            }
        }
        return 0
    }
 }
```

### 解法四：

公式法：2(a+b+c)-(2a+2b+c)=c

```swift
 class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        return 2 * Set(nums).reduce(0, { (x, y) -> Int in
            x + y
        }) - nums.reduce(0, { (x, y) -> Int in
            x + y
        })
    }
 }
```

### 解法五：

超效率的异或大法。因为只有一个元素出现一次，其他的元素都出现两次，根据异或的性质，两个相同的元素异或为0，一个元素和0异或为当前元素，也就是所有相同的元素两两异或等于0，然后再和出现一次的元素异或，则等于该元素。

```swift
 class Solution {
    func singleNumber(_ nums: [Int]) -> Int {
        return nums.reduce(0, { (x, y) -> Int in
            x^y
        })
    }
 }
```
