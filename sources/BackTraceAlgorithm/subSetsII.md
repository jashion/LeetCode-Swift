给定一个可能包含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

```
示例:

输入: [1,2,2]
输出:
[
 [2],
 [1],
 [1,2,2],
 [2,2],
 [1,2],
 []
]
```

### 解法一：回溯法

和子集I一样，可以使用回溯法，但是需要去掉重复的子集。这里可以使用Set，不过nums数组需要变成有序。不然过不掉：[144]和[414]的情况。

```swift
 class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var res = Set<[Int]>()
        var sub = [Int]()
        backTrace(nums.sorted(), 0, &sub, &res)
        var result = Array(res)
        result.append([])
        return result
    }

    func backTrace(_ nums: [Int], _ i: Int, _ sub: inout [Int], _ res: inout Set<[Int]>) {
        var j = i
        while j < nums.count {
            sub.append(nums[j])
            res.insert(sub)
            backTrace(nums, j+1, &sub, &res)
            sub.removeLast()
            j += 1
        }
    }
 }
```

不实用Set，使用数组。

```swift
 //为什么要先排序？这样才能保证相同的数字在一起
 //比如说：1,2,4,4  124(第2位4)和124(第3位4)就可以使用nums[i] == nums[i-1]来判断是否重复
 //为什么可以这样判断？因为每次回溯，都会移除nums[i]，然后符合条件就添加nums[i+1]，那么回溯前[nums[0]~nums[i]]，回溯后[nums[0]~nums[i-1]nums[i+1]]，假如nums[i]=nums[i+1]，那么，前面两个集合就会重复了，所以可以直接跳过
 class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var res: [[Int]] = [[]]
        var sub = [Int]()
        backTrace(nums.sorted(), 0, &sub, &res)
        return res
    }

    func backTrace(_ nums: [Int], _ i: Int, _ sub: inout [Int], _ res: inout [[Int]]) {
        var j = i
        while j < nums.count {
            if j > i, nums[j] == nums[j-1] {
                j += 1
                continue
            }
            sub.append(nums[j])
            res.append(sub)
            backTrace(nums, j+1, &sub, &res)
            sub.removeLast()
            j += 1
        }
    }
 }
```

### 解法二：迭代法

迭代法（动态规划），先考虑0个数据的情况，然后在这基础上添加一个数据，然后再添加一个数据：

nums: [1, 2, 3]

i = 0  res: [[]]

i = 1  res: [[], [1]]

i = 2  res: [[], [1], [2], [12]]

i = 3  res: [[], [1], [2], [12], [3], [13], [23], [123]]

不过，需要考虑重复的情况：

nums: [1, 2, 2]

i = 0 res: [[]]

i = 1 res: [[], [1]]

i = 2 res: [[], [1], [2], [12]]

i = 3 res: [[], [1], [2], [12], [2], [12], [22], [122]]

因为第2个2和第1个2重复了，所以，[2], [12]其实可以去掉，也就是前面一个2的所产生的解。【[详细参考]([https://leetcode-cn.com/problems/subsets-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-19/](https://leetcode-cn.com/problems/subsets-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-19/)】

```swift
 class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var res: [[Int]] = [[]]
        var i = 0
        var start = 1
        let numsSort = nums.sorted()
        while i < numsSort.count {
            var j = 0
            var resTmp = [[Int]]()
            while j < res.count {
                if i > 0, numsSort[i] == numsSort[i-1], j < start {
                    j += 1
                    continue
                }
                var tmp = res[j]
                tmp.append(numsSort[i])
                resTmp.append(tmp)
                j += 1
            }
            start = res.count
            res.append(contentsOf: resTmp)
            i += 1
        }
        return res
    }
 }
```

还有一种解法，就是单独考虑重复数字的情况：

比如：[1 2 2 2]

[[], [1]]

然后对于：[]

增加1个 2  [2]

增加2个 2  [2 2]

增加3个 2  [2 2 2]

对于：[1]

增加1个 2 [1 2]

增加2个 2 [1 2 2]

增加3个 2 [1 2 2 2]

```swift
 class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var res: [[Int]] = [[]]
        let numsSort = nums.sorted()
        var i = 0
        while i < numsSort.count {
            var dupCount = 0
            while (i+1) < numsSort.count, numsSort[i+1] == numsSort[i] {
                dupCount += 1
                i += 1
            }

            var prevNum = res.count
            for j in 0..<prevNum {
                var element = res[j]
                for t in 0...dupCount {
                    element.append(numsSort[i])
                    res.append(element)
                }
            }
            i += 1
        }
        return res
    }
 }
```

### 解法三：二进制法

每一个数都可以由一个二进制表示，1添加，0不添加，所以，总共有1<<nums.length个数，逐一添加进结果数组就可以了。不过需要去掉重复的组合。

比如：

2 2 2 2 2

1 0 0 0 0   ->  2

1 1 0 0 0   ->  2 2

1 1 1 0 0   ->  2 2 2

1 1 1 1 0   ->  2 2 2 2

1 1 1 1 1   ->  2 2 2 2 2

只需要这几个组合，排除其他组合。【[详解]([https://leetcode-cn.com/problems/subsets-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-19/](https://leetcode-cn.com/problems/subsets-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-19/)】

```swift
 class Solution {
    func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
        var res = [[Int]]()
        let numsSort = nums.sorted()
        let subSetNum = 1 << numsSort.count
        for i in 0..<subSetNum {
            var tmp = [Int]()
            var illegal = false
            for j in 0..<numsSort.count {
                if (i>>j&1) == 1 {
                    if j > 0, numsSort[j] == numsSort[j - 1], i>>(j - 1)&1 == 0 {
                        illegal = true
                        break
                    } else {
                        tmp.append(numsSort[j])
                    }
                }
            }
            if !illegal {
                res.append(tmp)
            }
        }
        return res
    }
 }
```
