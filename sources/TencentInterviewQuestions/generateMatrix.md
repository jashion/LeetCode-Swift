### 题目：

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

```
示例:

输入: 3
输出:
[
[ 1, 2, 3 ],
[ 8, 9, 4 ],
[ 7, 6, 5 ]
]
```

### 解法一：

将整个矩阵分成一个一个圈，比如n=3就有一个外圈和一个中心点，n=4有两个圈，没有中心点，也就是说n为奇数时有中心点，而圈的数量为n/2。

```swift
 class Solution {
    func generateMatrix(_ n: Int) -> [[Int]] {
        var res = Array.init(repeating: Array.init(repeating: 0, count: n), count: n)
        var i = 1
        var j = 1
        while i <= n/2 {
            var row = i-1
            var column = i-1
            while column <= n-i {
                res[row][column] = j
                column += 1
                j += 1
            }
            column -= 1
            row += 1
            while row <= n-i {
                res[row][column] = j
                row += 1
                j += 1
            }
            row -= 1
            column -= 1
            while column >= i-1 {
                res[row][column] = j
                column -= 1
                j += 1
            }
            column += 1
            row -= 1
            while row >= i {
                res[row][column] = j
                row -= 1
                j += 1
            }
            i += 1
        }
        if n%2 != 0 {
            res[n/2][n/2] = n * n
        }
        return res
    }
 }
```

### 解法二：

思路一样，代码更为精简。

```swift
 class Solution {
    func generateMatrix(_ n: Int) -> [[Int]] {
        var l = 0, r = n - 1, t = 0, b = n - 1
        var mat = Array.init(repeating: Array.init(repeating: 0, count: n), count: n)
        var num = 1, tar = n * n
        while num <= tar {
            var i = l
            while i <= r {
                mat[t][i] = num
                num += 1
                i += 1
            }
            t += 1
            i = t
            while i <= b {
                mat[i][r] = num
                num += 1
                i += 1
            }
            r -= 1
            i = r
            while i >= l {
                mat[b][i] = num
                num += 1
                i -= 1
            }
            b -= 1
            i = b
            while i >= t {
                mat[i][l] = num
                num += 1
                i -= 1
            }
            l += 1
        }
        return mat
    }
 }
```
