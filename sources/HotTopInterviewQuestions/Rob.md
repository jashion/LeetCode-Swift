### 题目：

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

示例 1:

```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```
示例 2:

```
输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

### 解法一：动态规划

动态规划三个条件：

1.最优子结构：每一个房屋存在抢或者不抢的状态，假设当前房子为第i间，如果抢抢该房子，那么就不能抢第i-1间房，如果不抢该房子，则可以抢i-1间房，所以，d[i] = Max(d[i-1], d[i-2]+nums[i])

2.边界：当n=0时，res=0，当n=1时，res=nums[0]，当n=2时，res=Max(nums[0], nums[1])

3.状态转移方程：

res = {

0  (n<=0)

nums[0]  (n=1)

Max(d[n-2], d[n-3]+nums[n])  (n>1)

}

```swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        if nums.count == 0 {
            return 0
        }
        if nums.count == 1 {
            return nums[0]
        }
        if nums.count == 2 {
            return max(nums[0], nums[1])
        }
        var preRes = nums[0]
        var curRes = max(nums[0], nums[1])
        for i in 2...nums.count-1 {
            let tmp = max(curRes, preRes+nums[i])
            preRes = curRes
            curRes = tmp
        }
        return curRes
    }
}
```

### 解法二：奇偶法

当第一眼看到这道题就到了，将所有奇数累加和所有偶数累加，然后返回最大的那个数。但是，还有可能是有奇有偶，不能直接这样计算。但是，有人解决了这个问题，就是，当奇数累加的值比偶数累加的值小，则偶数累加的值替换奇数累加的值，反之依然。看到这个思路，眼前为之一亮，佩服。

```swift
class Solution {
    func rob(_ nums: [Int]) -> Int {
        var oddSum = 0
        var evenSum = 0
        for i in 0..<nums.count {
            if i%2 == 0 {
                evenSum += nums[i]
                evenSum = max(evenSum, oddSum)
            } else {
                oddSum += nums[i]
                oddSum = max(evenSum, oddSum)
            }
        }
        return max(oddSum, evenSum)
    }
}
```


