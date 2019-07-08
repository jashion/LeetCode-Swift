### 题目：

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

```
示例 1:

输入: 123
输出: 321
 示例 2:

 输入: -123
 输出: -321
 示例 3:

 输入: 120
 输出: 21
 注意:

 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0
```

 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

### 解法一：

直接使用数字翻转，也就是原数字Pop一位，res就Push一位，在这工程需要注意溢出的问题。

```swift
 class Solution {
    func reverse(_ x: Int) -> Int {
        var result:Int = 0
        var tempValue:Int = x
        while tempValue != 0 {
            result = result*10 + tempValue%10
            tempValue = tempValue/10
        }
        if result < Int32.min || result > Int32.max {
            result = 0
        }
        return result
    }
 }
```

### 解法二：

数字翻转解法二。

```swift
 class Solution {
    func reverse(_ x: Int) -> Int {
        var result:Int = 0
        var tempValue:Int = x
        let min = Int32.min
        let max = Int32.max
        while tempValue != 0 {
            result = result*10 + tempValue%10
            tempValue = tempValue/10
            if result < min || result > max {
                return 0
            }
        }
        return result
    }
 }
```

### 解法三：

数字翻转解法三。

```swift
 class Solution {
    func reverse(_ x: Int) -> Int {
        var res:Int = 0
        var temp:Int = x
        let min = Int32.min
        let max = Int32.max
        while temp != 0 {
            let pop = temp % 10
            temp /= 10
            if res > max / 10 || (res == max / 10 && pop > 7){
                return 0
            }
            if res < min / 10 || (res == min / 10 && pop < -8) {
                return 0
            }
            res = res * 10 + pop
        }
        return res
    }
 }
```

### 解法四：

字符串翻转法，将整个数字转换成数字，翻转，再累加。

```swift
 class Solution {
    func reverse(_ x: Int) -> Int {
        if x == 0 {
            return x
        }
        let negetive = x > 0 ? false : true
        let arr = Array("\(abs(x))")
        var res = 0
        for item in arr.reversed() {
            res *= 10
            res += Int(String(item))!
        }
        res = negetive ? -res : res
        if res < Int32.min || res > Int32.max {
            res = 0
        }
        return res
    }
 }
```

### 解法五：

字符串翻转法二。

```swift
 class Solution {
    func reverse(_ x: Int) -> Int {
        if x == 0 {
            return x
        }
        let negetive = x > 0 ? false : true
        var res = "\(abs(x))".reversed().map{Int(String($0))}.reduce(0, {x, y in
            x*10 + y!
        })
        res = negetive ? -res : res
        if res < Int32.min || res > Int32.max {
            res = 0
        }
        return res
    }
 }
```
