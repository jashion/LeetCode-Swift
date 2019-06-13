### 题目：

 给定一个数组，它的第 *i*  个元素是一支给定股票第  *i*  天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 解法一：暴力法

计算所有可能的交易组合的相对利益，并找出最大值。

比如：[7, 1, 5, 3, 6, 4]

[1, 5], [1, 3], [1, 6], [1, 4], [5, 6], [3, 6], [3, 4]

相对的组合就有：

[1, 5], [3, 6] = 7

[1, 5], [3, 4] = 5

[1, 3] = 2

[1, 6] = 5

...

选取最大的值就：7就是最大利益。

```swift
//刚开始会对[1, 2], [3, 4]这样的组合有疑问？因为很明显[1, 4]利益更大，但是这只是在[1, 2]已经发生的情况下的其中一种组合，其实还有在[1, 4]已经发生的另外的组合，所以不冲突
//关于时间复杂度，因为递归的深度为O(n)（因为只有n个数），每次都是遍历O(n)，也就是O(n)*O(n)*O(n)...=O(n^n)。
//关于空间复杂度，因为递归的深度为O(n)，并且只用到了常数级的局部变量，所以空间复杂度也为O(n)。
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        return calculate(prices, 0)
    }

    func calculate(_ prices: [Int], _ s: Int) -> Int {
        guard s < prices.count else {
            return 0
        }

        var max = 0
        for start in s..<prices.count {
            var maxProfit = 0
            for i in s+1..<prices.count {
                if prices[start] < prices[i] {
                    let profit = calculate(prices, i+1) + prices[i] - prices[start]
                    if profit > maxProfit {
                        maxProfit = profit
                    }
                }
            }
            if maxProfit > max {
                max = maxProfit
            }
        }
        return max
    }
}
```

### 解法二：波峰波谷法

找到最近的波谷和波峰，类似于显示股票的低买高卖。

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }
        var valley = 0
        var peek = 0
        var maxProfit = 0
        for _ in peek..<prices.count {
            for j in peek..<prices.count {
                if j == prices.count-1 {
                    valley = prices.count-1
                    break
                } else if prices[j+1] > prices[j] {
                    valley = j
                    break
                }
            }
            if valley >= prices.count-1 {
                break
            }
            for k in valley..<prices.count {
                if k == prices.count - 1 {
                    peek = prices.count - 1
                    break
                } else if prices[k+1] < prices[k]  {
                    peek = k
                    break
                }
            }
            maxProfit = maxProfit + prices[peek]-prices[valley]
            if peek >= prices.count - 1 {
                break
            }
        }
        return maxProfit
    }
}
```

###### 代码精简：

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }
        var valley = prices[0]
        var peek = prices[0]
        var maxProfit = 0
        var i = 0
        while i < prices.count - 1 {
            while i < prices.count - 1, prices[i] >= prices[i+1] {
                i = i + 1
            }
            valley = prices[i]
            while i < prices.count - 1, prices[i] <= prices[i+1] {
                i = i + 1
            }
            peek = prices[i]
            maxProfit = maxProfit + peek - valley
        }
        return maxProfit
    }
}
```

### 解法三：连续波峰法

和解法二的思路一样，也是寻找波谷和波峰，只不过相邻的两个波谷和波峰，而是波谷和相邻的最大波峰。

```swift
//使用元组
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count >= 1 else {
            return 0
        }
        var profit = 0
        var inIndex = (false, -1)
        for i in 0..<prices.count {
            if !inIndex.0 {
                if i < prices.count-1 {
                    let tmp1 = prices[i]
                    let tmp2 = prices[i+1]
                    if tmp1 < tmp2 {
                        inIndex = (true, tmp1)
                    }
                }
            } else {
                let tmp1 = prices[i-1]
                let tmp2 = prices[i]
                if tmp2 < tmp1 {
                    profit = profit + tmp1-inIndex.1
                    inIndex = (true, tmp2)
                } else {
                    if i == prices.count-1 && inIndex.0 {
                        profit = profit + tmp2 - inIndex.1
                    }
                }
            }
        }

        return profit
    }
}

//其他
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count >= 1 else {
            return 0
        }
        var profit = 0
        var fixIndex: [[Int: Int]] = []
        for i in 0..<prices.count-1 {
            if prices[i] > prices[i+1] {
                fixIndex.append([i: prices[i]])
            }
        }
        for i in 0..<fixIndex.count {
            if i == 0 {
                profit = profit + fixIndex[0].values.first!-prices[0]
            } else {
                let tmp = prices[fixIndex[i-1].keys.first!+1]
                profit = profit + fixIndex[i].values.first!-tmp
            }
        }
        if fixIndex.count == 0 {
            profit = prices.last!-prices.first!
        } else if fixIndex.last?.first?.key != prices.count-1  {
            profit = profit + prices.last! - prices[(fixIndex.last?.keys.first)!+1]
        }
        return profit
    }
}
```

###### 代码精简：

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }
        var profit = 0
        for i in 1..<prices.count {
            if prices[i] > prices[i-1] {
                profit = profit + prices[i] - prices[i-1]
            }
        }
        return profit
    }
}
```

### 解法四：比较绕，看了写了好多遍才理解

以数组：[7, 1, 5, 3, 6, 4]为例

首先第一天，因为之前没有持有股票，所以dp[0][0]=0，收益为0，假如持有股票，则dp[0][1]=-7，收益为-7。

第二天，假如第一天持有股票，第二天卖出，则收益为-7+1=-6，和之前的利益相比，很明显利益还是0。那么，如果第一天不买入，第二天买入则Max(-7, 0-1)，很明显第二天买入会更好，所以，dp[1][0]=0，dp[1][1]=-1。

第三天，同理，第三天股票卖出，则收益为-1+5=4（因为dp[i][0]表示股票在昨天之前已经卖出的利益，dp[i][1]则表示股票还持有的利益），因为Max(4, 0)，所以d[2][0]=4，如果昨天没有买入，今天买入则0-5=-5，Max(-1, -5)，很明显dp[2][1]=-1。

第四天。同理，股票卖出，则收益为-1+3=2，Max(4, 2)=4，则dp[3][0]=4，如果昨天没有买入，今天买入则4-3=1，Max(-1, 1)=1，所以dp[3][1]=1。

第五天，同理，股票卖出，则收益为1+6=7，Max(7, 4)=7，则dp[4][0]=7，如果昨天没有买入，今天买入则4-6=-2，Max(-2, 1)=1，则dp[4][1]=1。

第六天，同理，股票卖出，则收益为1+4=5，Max(7, 5)=7，则dp[5][0]=7，如果昨天没有买入，今天买入则7-4=3，Max(3, 1)=3，则dp[7][1]=3。

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        if prices.isEmpty {
            return 0
        }
        var maxProfit = 0
        var dp : [[Int]] = Array<[Int]>(repeating: Array<Int>(repeating: 0, count: 2), count: prices.count)
        dp[0][0] = 0 //dp[i][0]为不持有当天股票的利润
        dp[0][1] = -prices[0] //dp[i][1]代表持有当天股票时的利润
        print(dp)
        for i in 1..<prices.count {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i]) //dp[i - 1][1] + prices[i]：表示昨天不卖出，今天卖出的利润。如果当前价格卖出收益更大，则卖出
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]) //dp[i - 1][0] - prices[i]：表示昨天卖出，今天买入的利润。利润比之前的高，说明当天的价格比昨天的地，否则，当天的价格比昨天的高，不买入
            print(dp)
            if dp[i][0] > maxProfit {
                maxProfit = dp[i][0]
            }
        }
        return maxProfit
    }
}
```
