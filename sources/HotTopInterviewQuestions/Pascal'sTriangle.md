### 题目：

给定一个非负整数 *numRows，*生成杨辉三角的前 *numRows* 行。比如：numRows=5，则输出：[[1], [1, 1], [1, 2, 1], [1, 3, 3, 1], [1, 4, 6, 4, 1]]。

### 解题思路：

杨辉三角的规律为：第一个和最好一个数为1，其余的数为上一个数列的相同下标和前面一个下标之和。

```swift
class Solution {
    func generate(_ numRows: Int) -> [[Int]] {
        var result: [[Int]] = []
        for i in 0..<numRows {
            var temp: [Int] = [1]
            if i > 1 {
                let topNums = result[i-1]
                for j in 1..<i {
                    temp.append(topNums[j-1]+topNums[j])
                }
            }
            if i > 0 {
                temp.append(1)
            }
            result.append(temp)
        }
        return result
    }
}
```
