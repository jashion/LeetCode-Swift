### 题目：

报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

```
1.1

2.11

3.21

4.1211

5.111221

1 被读作 "one 1" ("一个一") , 即 11。
11 被读作 "two 1s" ("两个一"）, 即 21。
21 被读作 "one 2", "one 1" （"一个二" , "一个一") , 即 1211。
```

给定一个正整数 n（1 ≤ n ≤ 30），输出报数序列的第 n 项。

注意：整数顺序将表示为一个字符串。

### 解题思路：

通过遍历前面的结果，统计处相连的相同数字的个数，然后生成相应的字符串。

###### 解法一：直接使用整形数组

```swift
class Solution {
    func countAndSay(_ n: Int) -> String {
        guard n > 1 else {
            if n == 1 {
                return "1"
            } else {
                return ""
            }
        }

        var result: [Int] = [1]
        for _ in 2...n {
            var tempResult: [Int] = []
            var i = 0
            var indicate = 1
            while i < result.count {
                let temp = result[i]
                if i < result.count - 1, temp == result[i+1]{
                    indicate = indicate + 1
                } else {
                    tempResult.append(indicate)
                    tempResult.append(temp)
                    indicate = 1
                }
                i = i + 1
            }
            result = tempResult
        }

        return  result.map { (value) -> String in
            "\(value)"
            }.joined(separator: "")

    }
}
```

###### 解法二：使用字符串，并且使用迭代的方式

```swift
class Solution {
    func countAndSay(_ n: Int) -> String {
        var num = "1"

        if n == 1 {
            return num
        }

        for _ in 1..<n {
            var newStr = String()
            let arrayOfStr = Array(num)
            var count = 1

            for i in 0..<arrayOfStr.count {
                if i == arrayOfStr.count - 1 {
                    newStr += "\(count)\(arrayOfStr[i])"
                } else {
                    if arrayOfStr[i] == arrayOfStr[i + 1] {
                        count += 1
                    } else {
                        newStr += "\(count)\(arrayOfStr[i])"
                        count = 1
                    }
                }
            }
            num = newStr
        }
        return num
    }
}
```

###### 解法三：使用字符串，使用递归的方式

```swift
class Solution {
    func countAndSay(_ n: Int) -> String {
        if n == 1 { return "1" }
        let array = Array(countAndSay(n - 1))
        var res = ""
        var count = 1
        for i in 0..<array.count - 1 {
            if array[i] == array[i + 1] {
                count += 1
            } else {
                res = "\(res)\(count)\(array[i])"
                count = 1
            }
        }
        return "\(res)\(count)\(array.last!)"
    }
}
```
