### 题目：

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

### 解题思路：

遍历字符串，符合特殊的字符串组合就累加相应的值，否则按照普遍的规则累加数值。

```swift
class Solution {
    func romanToInt(_ s: String) -> Int {
        let specialRomanValues: [String: Int] = ["IV": 4, "IX": 9, "XL": 40, "XC": 90, "CD": 400, "CM": 900];
        for (key, value) in specialRomanValues {
            if key == s {
                return value
            }
        }
        let romanValues = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]
        let specialKeys = ["I", "X", "C"]
        var sum:Int = 0
        var tempStr: String = ""
        for (index, value) in s.enumerated() {
            let str = String(value)
            if tempStr.isEmpty, specialKeys.contains(str) {
                tempStr.append(str)
            } else if !tempStr.isEmpty {
                tempStr.append(str)
            }
            if tempStr.isEmpty {
                if let item = romanValues[str] {
                    sum = sum + item
                }
            } else {
                if tempStr.count == 2 {
                    if let item = specialRomanValues[tempStr] {
                        sum = sum + item
                        tempStr.removeAll()
                    } else {
                        print(tempStr+": \(index)")
                        for (tempIndex, tempValue) in tempStr.enumerated() {
                            let item2 = String(tempValue)
                            if tempIndex == 0 {
                                if let item = romanValues[item2] {
                                    sum = sum + item
                                }
                            }
                        }
                        tempStr.remove(at: tempStr.startIndex)
                        if index == s.count-1 {
                            if let item = romanValues[String(tempStr)] {
                                sum = sum + item
                            }
                        }
                    }
                } else {
                    if index == s.count-1 {
                        if let item = romanValues[String(tempStr)] {
                            sum = sum + item
                        }
                    }

                }
            }
        }
        return sum
    }
}
```

### 算法改进：

因为一般左边的数字小于右边的数字，只有特殊的情况下才会有左边的数字大于右边的数字，并且一般情况下直接累加，特殊情况下左边的数字减去右边数字，按照这个规律改进算法。

```swift
class Solution {
    func romanToInt(_ s: String) -> Int {
        let romanValues = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]
        var sum:Int = 0
        for index in s.indices {
            let lastIndex = s.index(before: s.endIndex)
            if index < lastIndex {
                let nextIndex = s.index(after: index)
                if romanValues[String(s[index])]! < romanValues[String(s[nextIndex])]! {
                    sum = sum - romanValues[String(s[index])]!
                } else {
                    sum = sum + romanValues[String(s[index])]!
                }
            } else {
                sum = sum + romanValues[String(s[index])]!
            }
        }
        return sum
    }
}
```

### 另外的解法1：

和上面的思路一样，左边的数字小于右边，则减去左边的数字，左边的数字大于右边，则加上左边的数字。

```swift
class Solution {
    func transfromCharaterToInt(_ c: Character) -> Int {
        switch c {
        case "I":
            return 1
        case "V":
            return 5
        case "X":
            return 10
        case "L":
            return 50
        case "C":
            return 100
        case "D":
            return 500
        case "M":
            return 1000

        default:
            return 0
        }
    }

    func romanToInt(_ s: String) -> Int {
        var sum = 0
        var pre: Character?
        for value in s.reversed() {
            if value == "I" && pre != nil && (pre == "V" || pre == "X") {
                sum = sum - 1
            } else if value == "X" && pre != nil && (pre == "L" || pre == "C") {
                sum = sum - 10
            } else if value == "C" && pre != nil && (pre == "D" || pre == "M") {
                sum = sum - 100
            } else {
                sum = sum + transfromCharaterToInt(value)
                pre = value
            }
        }
        return sum
    }
}
```

### 另外的解法2：

将所有的罗马字符都用普通的处理，然后遇到特殊的减去双倍就OK了。

```swift
class Solution {
    func transfromCharaterToInt(_ c: Character) -> Int {
        switch c {
        case "I":
            return 1
        case "V":
            return 5
        case "X":
            return 10
        case "L":
            return 50
        case "C":
            return 100
        case "D":
            return 500
        case "M":
            return 1000

        default:
            return 0
        }
    }

    func romanToInt(_ s: String) -> Int {
        var sum = 0
        var pre: Character?
        for value in s.reversed() {
            if pre != nil && transfromCharaterToInt(value) < transfromCharaterToInt(pre!) {
                sum = sum - transfromCharaterToInt(value)*2
            }
            sum = sum + transfromCharaterToInt(value)
            pre = value
        }
        return sum
    }
}
```
