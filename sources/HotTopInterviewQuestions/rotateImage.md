### 题目：

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

```
示例 1:

给定 matrix = 
[
[1,2,3],
[4,5,6],
[7,8,9]
],

原地旋转输入矩阵，使其变为:
[
[7,4,1],
[8,5,2],
[9,6,3]
]
示例 2:

给定 matrix =
[
[ 5, 1, 9,11],
[ 2, 4, 8,10],
[13, 3, 6, 7],
[15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
[15,13, 2, 5],
[14, 3, 4, 1],
[12, 6, 8, 9],
[16, 7,10,11]
]
```

### 解法一：

可以先实现最简单的解法，不用考虑只能使用原矩阵的情况。

```swift
 class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        guard matrix.count >= 2 else {
            return
        }

        var res = [[Int]]()
        for i in 0..<matrix.count {
            var tmp = [Int]()
            for j in (0..<matrix.count).reversed() {
                tmp.append(matrix[j][i])
            }
            res.append(tmp)
        }
        matrix = res
    }
 }
```

### 解法二：

考虑只能在原矩阵修改，从事例可以得到，顺时针旋转90度，也就是第一行的数据变成最后一列的数据，问题的关键是，调整原矩阵而不丢失原数据。以事例一为例，对角1，3，9，7变成7，1，3，9也就是1和3交换，3和9交换，9和7交换，7和一交换，余下的2，6，8，9也是一样的情况。并且，第一行1，2，3其实只需交换1，2，因为1和3会直接交换。可以把整个矩阵看作多个全套的矩形，比如事例一，有一个矩形[1, 2, 3, 6, 9, 8, 7, 4]，其中5不构成矩形，所以不用动；事例二，有两个矩形[5, 1, 9, 11, 10, 7, 16, 12, 14, 15, 13, 2]和[4, 8, 6, 3]，所以需要调整两次，找到规律就可以敲代码了。

```swift
 class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        guard matrix.count >= 2 else {
            return
        }
        let length = matrix.count
        let n = length/2
        for i in 0..<n {
            let count = length-1-i
            var j = i
            while j < count {
                for k in 1...3 {
                    var tmp = matrix[i][j]
                    if k == 1 {
                        matrix[i][j] = matrix[j][length-1-i]
                        matrix[j][length-1-i] = tmp
                    } else if k == 2 {
                        matrix[i][j] = matrix[length-1-i][length-1-j]
                        matrix[length-1-i][length-1-j] = tmp
                    } else {
                        matrix[i][j] = matrix[length-1-j][i]
                        matrix[length-1-j][i] = tmp
                    }
                }
                j += 1
            }
        }
    }
 }
```

以事例一为例，上面的做法是，先交换1和3，然后再交换3和9，最后交换9和7。

```swift
 class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        let n = matrix.count
        for i in 0..<n/2 {
            for j in i..<n - i - 1 {
                let tmp = matrix[i][j]
                matrix[i][j] = matrix[n-j-1][i]
                matrix[n-j-1][i] = matrix[n-i-1][n-j-1]
                matrix[n-i-1][n-j-1] = matrix[j][n-i-1]
                matrix[j][n-i-1] = tmp
            }
        }
    }
 }
```

上面的做法也类似。

### 解法三：

顺时针旋转90度和先将矩阵关于对角线对称，然后再倒序是一样的效果。

```swift
 class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        guard matrix.count >= 2 else {
            return
        }

        let n = matrix.count
        for i in 0..<n {
            for j in i..<n {
                let tmp = matrix[j][i]
                matrix[j][i] = matrix[i][j]
                matrix[i][j] = tmp
//                matrix[i][j] = matrix[i][j] + matrix[j][i]
//                matrix[j][i] = matrix[i][j] - matrix[j][i]
//                matrix[i][j] = matrix[i][j] - matrix[j][i]
            }
        }

        for i in 0..<n {
            matrix[i] = matrix[i].reversed()
        }
        
//        for i in 0..<n {
//            for j in 0..<n/2 {
//                let tmp = matrix[i][j]
//                matrix[i][j] = matrix[i][n-1-j]
//                matrix[i][n-1-j] = tmp
//            }
//        }
    }
 }
```
