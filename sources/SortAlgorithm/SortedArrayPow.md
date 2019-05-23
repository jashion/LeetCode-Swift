### 题目：

给定一个按非递减顺序排序的整数数组 `A`，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

### 解题思路：

声明三个变量i=0，j=A.count-1和index=A.count-1，i下标从0开始遍历，j下标从A.count-1开始遍历，比较A[i]平方和A[j]平方的大小，将大的加入结果数组，并且相应的下标往前或者往后移动一位，直到i=j，则result[0]=A[i]*A[i]。

```swift
class Solution {
    func sortedSquares(_ A: [Int]) -> [Int] {
        if A.count == 0 {
            return []
        }
        if A.count == 1 {
            return [A[0]*A[0]]
        }
        var i = 0
        var j = A.count-1
        var index = j
        var result = Array.init(repeating: 0, count: A.count);
        while i < j {
            let valueI = A[i]*A[i]
            let valueJ = A[j]*A[j]
            if valueI > valueJ {
                result[index] = valueI
                i = i+1
            } else {
                result[index] = valueJ
                j = j-1
            }
            index = index-1
        }
        result[index] = A[i]*A[i]
        return result
    }
}
```

### 更简洁的解法：

因为是有序，可以先平方再排序。

```swift
class Solution {
    func sortedSquares(_ A: [Int]) -> [Int] {
        var num = A.map{$0*$0}
        num.sort {$0<$1}
        return num
    }
}
```


