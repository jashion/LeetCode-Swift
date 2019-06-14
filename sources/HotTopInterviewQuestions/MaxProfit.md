### 题目：

 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

示例 1:

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```
示例 2:

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

### 解法一：暴力法

计算所有组合的利益，取最大的利益。

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }
        var maxProfit = 0
        for i in 0..<prices.count {
            for j in i+1..<prices.count {
                maxProfit = max(maxProfit, prices[j]-prices[i])
            }
        }
        return maxProfit
    }
}
```

### 解法二：

从后往前遍历，寻找波峰，然后再从前往后遍历，计算每个波峰的最大利益，取所有波峰的最大利益。

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }

        var maxProfit = 0
        var index = prices.endIndex-1
        var tempMaxNum = prices[index]
        var maxNum: [[Int:Int]] = [[index: tempMaxNum]]
        while index > 0 {
            let value = prices[index]
            if value > tempMaxNum {
                tempMaxNum = value
                maxNum.append([index: tempMaxNum])
            }
            index -= 1
        }
        //比如：[5: 6]则表示从下标5开始，后面的数都比6小
        for i in 0..<prices.count-1 {
            let value = prices[i]
            if let item = maxNum.last {
                if i < item.keys.first! {
                    let result = item.values.first! - value
                    if result > maxProfit {
                        maxProfit = result
                    }
                } else {
                    maxNum.removeLast()
                }
            }
        }
        return maxProfit
    }
}
```

### 解法三：波峰波谷法

寻找最近的波谷，然后计算后面数值和波谷的差值，如果遇到比当前波谷更小的值，赋值给当前的波谷，然后继续计算后面数值和波谷的差值，选择最大的数值。

```swift
class Solution {
    func maxProfit(_ prices: [Int]) -> Int {
        guard prices.count > 0 else {
            return 0
        }
        var maxProfit = 0
        var minPrice = Int.max
        for i in 0..<prices.count {
            if prices[i] < minPrice {
                minPrice = prices[i]
            } else {
                maxProfit = max(maxProfit, prices[i] - minPrice)
            }
        }
        return maxProfit
    }
}
```
