### 题目：

给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。

示例 1:

```
输入: [3,0,1]
输出: 2
```
示例 2:

```
输入: [9,6,4,2,3,5,7,0,1]
输出: 8
```
说明:
你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?

### 解法一：

使用集合，先把nums里面的数都加入集合，然后遍历数组，判断是否不在集合里面的，不在合集里面就是缺失的数。

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        let temp = Set.init(nums)
        for i in 0..<nums.count {
            if !temp.contains(i) {
                return i
            }
        }
        return nums.count
    }
}
```

### 解法二：

计算0～n的和并且和nums所有数的和比较，找出缺失的数。

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        var missSum = 0
        var realSum = nums.count
        for i in 0..<nums.count {
            missSum = missSum + nums[i]
            realSum = realSum + i
        }
        return realSum-missSum
    }
}
```

###### 代码精简：

使用求和公式。

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        var res = (1+nums.count)*nums.count/2
        for num in nums {
            res = res - num
        }
        return res
    }
}
```

### 解法三：

非一般人解法，使用异或，成对的相等数字都会变成0，最后剩下一个独立的数字就是缺失的数字。

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        var missSum = nums.count
        for (i, value) in nums.enumerated() {
            missSum = missSum^value
            missSum = missSum^i
        }
        return missSum
    }
}
```
