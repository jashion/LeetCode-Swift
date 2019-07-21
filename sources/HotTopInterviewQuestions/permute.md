### 题目：

给定一个没有重复数字的序列，返回其所有可能的全排列。

示例:

```
输入: [1,2,3]
输出:
[
[1,2,3],
[1,3,2],
[2,1,3],
[2,3,1],
[3,1,2],
[3,2,1]
]
```

### 解法一：动态规划

边界：

当n=1时，res = [nums[0]]

当n>1时，遍历res(n-1)每一个解，然后在每一个解的0~(n-1)的位置插入nums[n-1]，得出的所有解就是res(n)的解

```swift
 class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var res: [[Int]] = [[]]
        for cur in nums {
            var tmp = [[Int]]()
            for item in res {
                for j in 0...item.count {
                    var tmp2 = item
                    tmp2.insert(cur, at: j)
                    tmp.append(tmp2)
                }
            }
            res = tmp
        }
        return res
    }
 }
```

### 解法二：回溯法

回溯法一，nums[1, 2, 3]可以得出下面一棵树：

```
                root
             /    |    \
            1     2     3
           / \   / \   / \
          2   3  1  3  2  1
          |   |  |  |  |  |
          3   2  3  1  1  2
```

代码如下：

每次访问了元素从数组移除，然后再访问移除后的数组。

```swift
 class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var res = [[Int]]()
        backTrace(nums, [], &res)
        return res
    }

    func backTrace(_ nums: [Int], _ sub: [Int], _ res: inout [[Int]]) {
        if nums.count == 0 {
            res.append(sub)
            return
        }

        var subSet = sub
        for i in 0..<nums.count {
            subSet.append(nums[i])
            var tmp = nums
            tmp.remove(at: i)
            backTrace(tmp, subSet, &res)
            subSet.removeLast()
        }
    }
 }
```

回溯法二：不通过移除元素，通过交换元素，每次被访问的元素都放在前面，然后从还没被访问的元素开始下次访问。(可以参考leetCode的[官方解析]([https://leetcode-cn.com/problems/permutations/solution/quan-pai-lie-by-leetcode/](https://leetcode-cn.com/problems/permutations/solution/quan-pai-lie-by-leetcode/))

```swift
  class Solution {
     func permute(_ nums: [Int]) -> [[Int]] {
         var res = [[Int]]()
         var tmp = nums
         backTrace(tmp.count, &tmp, &res, 0)
         return res
     }

    func backTrace(_ n: Int, _ nums: inout [Int], _ res: inout [[Int]], _ first: Int) {
         if first == n {
             res.append(nums)
             return
         }

        var i = first
        while i < n {
            nums.swapAt(first, i)
            backTrace(n, &nums, &res, first+1)
            nums.swapAt(first, i)
            i += 1
        }
     }
  }
```
