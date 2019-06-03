### 题目：

给定一个Excel表格中的列名称，返回其相应的列序号。比如：A -> 1 B -> 2 AA -> 27等等。

### 解题思路：

将字符串转换成ASCII码，然后是26进制，A->1, B->2...Z->26。

```swift
class Solution {
    func titleToNumber(_ s: String) -> Int {
        let startNum = Int(UnicodeScalar("A").value)-1
        let maxNum = 26
        var total = 0
        for (i, value) in s.reversed().enumerated() {
            let num = Int(UnicodeScalar(String(value))!.value)-startNum
            let decimal = (pow(Decimal(maxNum), i) as NSDecimalNumber).intValue
            total = total + num*decimal
        }
        return total
    }
}
```

### 算法改进：

去掉逆序。

```swift
class Solution {
    func titleToNumber(_ s: String) -> Int {
    if s.count  == 0 { return 0 }
    
    var result: Int = 0
    var index: Int  = 0
    for str in s {
        let asscii = UnicodeScalar( String(str))
        result = Int((asscii!.value - 64)) + (result * 26)
        index = index + 1
    }
    
    return result
    }
}
```
