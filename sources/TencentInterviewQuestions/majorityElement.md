### 题目：

给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

```
示例 1:

输入: [3,2,3]
输出: 3
示例 2:

输入: [2,2,1,1,1,2,2]
输出: 2
```

### 解法一：

暴力法，计算每个元素出现的次数，找出出现次数大于⌊ n/2 ⌋的元素。时间复杂度为O(n2)，空间复杂度为O(1)。

```swift
 //暴力法
 class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        let majorityCount = nums.count / 2
        for num in nums {
            var count = 0
            for elem in nums {
                if elem == num {
                    count += 1
                }
            }
            if count > majorityCount {
                return num
            }
        }
        return -1
    }
 }
```

### 解法二：

哈希表法，通过以元素为key，元素出现的次数为value，构建一个哈希表，然后取出出现次数大于⌊ n/2 ⌋的元素。时间复杂度O(n)，空间复杂度O(n)。

```swift
 //哈希表
 class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var counts = [Int:Int]()
        for num in nums {
            if counts[num] == nil {
                counts[num] = 1
            } else {
                counts[num] = counts[num]! + 1
            }
        }

        var majority:(key:Int, value:Int)? = nil
        for item in counts {
            if majority == nil {
                majority = item
            } else {
                if item.value > majority!.value {
                    majority = item
                }
            }
        }
        if majority == nil {
            return -1
        }
        return majority!.key
    }
 }
```

### 解法三：

排序法，因为众数出现次数大于⌊ n/2 ⌋，也就是说排序后第n/2个元素一定是众数。

众数出现三种情况（排序后）：

1.最小值，则nums[0~n/2]一定都为为众数

2.最大值，则nums[n/2~n-1]一定都为众数

3.中间，则nums[i~n/2~j]一定都为众数

所以，nums[n/2]一定为众数。

排序的时间复杂度为O(nlogn)，所以排序法的时间复杂度为O(nlogn)，空间复杂度为 O(1)。

```swift
 //排序法
 class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        return nums.sorted()[nums.count/2]
    }
 }
```

### 解法四：

随机法，因为超过1/2的几率可以得到众数，最坏的情况，无限循环。时间复杂度O(♾)，空间复杂度O(1)。

```swift
 //随机法
 class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        let majorityCount = nums.count / 2
        while true {
            let candidate = nums[Int(arc4random())%(nums.count)]
            if countOccurences(nums, candidate) > majorityCount {
                return candidate
            }
        }
    }

    func countOccurences(_ nums: [Int], _ num: Int) -> Int {
        var count = 0
        for item in nums {
            if item == num {
                count += 1
            }
        }
        return count
    }
 }
```

### 解法五：

二分法，将大问题分解为子问题解决。时间复杂度O(nlogn)，空间复杂度O(nlogn)。

```swift
 //二分法
  class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        return majorityElementReverse(nums, 0, nums.count-1)
    }

    func majorityElementReverse(_ nums: [Int], _ lo: Int, _ hi: Int) -> Int {
        if lo == hi {
            return nums[lo]
        }

        let mid = lo + (lo + hi)/2
        let left = majorityElementReverse(nums, lo, mid)
        let right = majorityElementReverse(nums, mid+1, hi)

        if left == right {
            return left
        }

        let leftCount = countInRange(nums, left, lo, hi)
        let rightCount = countInRange(nums, right, lo, hi)

        return leftCount > rightCount ? left : right
    }

    func countInRange(_ nums: [Int], _ left: Int, _ lo: Int, _ hi: Int) -> Int {
        var count = 0
        for i in lo...hi {
            if nums[i] == left {
                count += 1
            }
        }
        return count
    }
  }
```

### 解法六：

牛的不行的Boyer-Moore投票法，将众数看作1，其他数看作-1，那么所有数加起来结果肯定>0，这个就是投票法的精髓。就比如数组：[7, 7, 5, 7, 5, 1, 7]，先把第一个数当作众数，那么刚开始为res = 1，2，1，2，1，0因为res<=0那么，证明第一个数不是众数，换下一个也是7，这时候res = 1，因为已经遍历完整个数组，并且res = 1，所以7是众数。时间复杂度为O(n)，空间复杂度为O(1)。

```swift
 //Boyer-Moore投票算法
  class Solution {
     func majorityElement(_ nums: [Int]) -> Int {
        var count = 0
        var majority = nums[0]
        for num in nums {
            if count == 0 {
                majority = num
            }
            count += num == majority ? 1 : -1
        }
        return majority
     }
  }
```
