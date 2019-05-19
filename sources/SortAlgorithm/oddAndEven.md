### 题目：

给定一个非负整数数组 `A`， A 中一半整数是奇数，一半整数是偶数。

对数组进行排序，以便当 `A[i]`  为奇数时，`i` 也是奇数；当 `A[i]` 为偶数时，  `i`  也是偶数。

你可以返回任何满足上述条件的数组作为答案。

### 解题思路：

先把数据的奇数和偶数分成两个数组，然后分别不断从两个数组取一个元素加入结果数组。

```swift
class Solution {
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var odd = [Int]()
        var even = [Int]()
        for num in A {
            if num%2 == 0 {
                even.append(num)
            } else {
                odd.append(num)
            }
        }
        var result = Array.init(repeating: 0, count: A.count)

        for (index, value) in odd.enumerated() {
            result[1+index*2] = value
        }

        for (index, value) in even.enumerated() {
            result[index*2] = value
        }
        return result
    }
}
```

### 更高效的解法：

使用奇偶双下标法，一次遍历就可以得到结果数据。

```swift
class Solution {
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var result = Array.init(repeating: 0, count: A.count)
        var odd = 0
        var even = 0
        for num in A {
            if num%2 == 0 {
                result[even*2] = num
                even = even+1
            } else {
                result[1+odd*2] = num
                odd = odd+1
            }
        }
        return result
    }
}
```

### 一个比较巧的解法：

使用双下标，下标i表示偶数下标，从0开始，每次+2；下标j表示奇数下标，从1开始，每次也+2。从i开始，不断+2，直到下标的值为奇数，然后换j开始遍历，不断+2，直到下标的值为偶数，则直接交换swapAt(i, j)，这时候又从i开始，直到遍历完整个数组，结束。

```swift
class Solution {
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var result = Array.init(A)
        var i = 0
        var j = 1
        while i < result.count && j < result.count {
            if result[i]%2 == 0 {
                i = i+2
            } else {
                while result[j]%2 != 0 && j < result.count {
                    j = j+2
                }
                if j < result.count {
                    result.swapAt(i, j)
                }
            }
        }
        return result
    }
}
```
