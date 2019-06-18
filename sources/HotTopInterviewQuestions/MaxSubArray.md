### 题目：

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

### 解法一：暴力法

计算出每一个数字的连续相加的最大值，其中整个数组最大连续子数组的和就是其中的最大值。

```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        if nums.count <= 0 {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        var maxSum = nums[0]
        for i in 0..<nums.count {
            var tmp = 0
            for j in i..<nums.count {
                tmp += nums[j]
                maxSum = max(maxSum, tmp)
            }
        }
        return maxSum
    }
}
```

### 解法二：动态规划

动态规划三大条件：

```
1.最优子结构：第n个数的最大和为max(maxSum, sum+n)
sum的计算->第i个数，sum = sum+nums[i]，sum = max(sum, nums[i])。
2.边界：n=0，f(n) = 0; n=1, f(n) = 1。
3.状态转移方程：
f(n) = {
    0 (n<=0)
    1 (n=1)
    max(sum, f(n-1)) (sum=sum+nums[n],sum=max(sum, nums[n]))
}
```

使用动态规划代码实现如下：

```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        if nums.count <= 0 {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        var maxSum = nums[0]
        var sum = nums[0]
        for i in 1..<nums.count {
            sum += nums[i]
            sum = max(sum, nums[i])
            maxSum = max(maxSum, sum)
        }
        return maxSum
    }
}
```

### 解法三：分治法

分治法的思想和动态规划有点像，都是将不能立即解决的问题分成可以解决的小问题来解决，但是分治法更多的是解决没有重叠子问题，每个子问题都是独立的存在。在该问题里，可以将整个数组分成左右两部分，连续的最大和只能是在左边或者在右边或者在中间，代码实现如下：

```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        guard nums.count > 0 else {
            return 0
        }
        if nums.count == 1 {
            return nums.first!
        }

        let mid = (nums.count-1)/2
        let maxSum = max(maxSubArray(Array(nums[0...mid])), maxSubArray(Array(nums[mid+1..<nums.count])))

        var leftSum = nums[mid] //左边从mid递减
        var sum = 0
        for i in (0...mid).reversed() {
            sum += nums[i]
            leftSum = max(sum, leftSum)
        }

        var rightSum = nums[mid+1]  //右边从mid+1递增
        sum = 0
        for i in mid+1..<nums.count {
            sum += nums[i]
            rightSum = max(sum, rightSum)
        }
        return max(maxSum, leftSum+rightSum)
    }
}
```
