### 题目：

给定长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

```
示例:

输入: [1,2,3,4]
输出: [24,12,8,6]
说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。

进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）
```

### 解法一：

暴力法。

```swift
 class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var res = [Int]()
        for i in 0..<nums.count {
            var tmp = nums
            tmp[i] = 1
            res.append(tmp.reduce(1, { (total, x) -> Int in
                total * x
            }))
        }
        return res
    }
 }
```

### 解法二：

累乘法。把nums[0]~nums[n-2]和nums[1]~nums[n-1]的乘积缓存起来，那么res[i] = nums[0]..nums[i-1]*nums[i+1]..nums[n-1]，如下列表：

| res      |         |         |     |           |           |
|:-------- |:-------:|:-------:|:---:|:---------:|:---------:|
| res[0]   | 1       | nums[1] | ... | nums[n-2] | nums[n-1] |
| res[1]   | nums[0] | 1       | ... | nums[n-2] | nums[n-1] |
| ...      | ...     | ...     | ... | nums[n-2] | nums[n-1] |
| res[n-2] | nums[0] | nums[1] | ... | 1         | nums[n-1] |
| res[n-1] | nums[0] | nums[1] | ... | nums[n-2] | 1         |

```swift
        var res = [Int]()
        var tmp = [(Int, Int)]()
        for i in 0..<nums.count-1 {
            if i == 0 {
                tmp.append((nums.first!, nums.last!))
            } else {
                tmp.append((tmp.last!.0*nums[i], tmp.last!.1*nums[nums.count-1-i]))
            }
        }
        for i in 0..<nums.count {
            if i == 0 {
                res.append(tmp.last!.1)
            } else if i == nums.count - 1 {
                res.append(tmp.last!.0)
            } else {
                res.append(tmp[i-1].0*tmp[nums.count-1-i-1].1)
            }
        }
        return res
    }
 }
```

### 解法三：

思路和解法二一致，实现再简洁一点，不用使用辅助数组。

```swift
 class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var res = [Int]()
        var tmp = 1
        for i in 0..<nums.count {
            res.append(tmp)
            tmp *= nums[i]
        }
        print(res)
        tmp = 1
        for i in (0..<nums.count).reversed() {
            res[i] *= tmp
            tmp *= nums[i]
        }
        return res
    }
 }
```

### 解法四：

思路和解法二一致，实现比再解法三的基础上更进一步。

```swift
 class Solution {
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        var res = Array.init(repeating: 1, count: nums.count)
        var left = 1
        var right = 1
        for i in 0..<nums.count {
            res[i] *= left
            left *= nums[i]

            res[nums.count-1-i] *= right
            right *= nums[nums.count-1-i]
        }
        return res
    }
 }
```
