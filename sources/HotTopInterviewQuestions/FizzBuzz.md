### 题目：

写一个程序，输出从 1 到  *n*  数字的字符串表示。

1. 如果 *n* 是3的倍数，输出“Fizz”；

2. 如果 *n* 是5的倍数，输出“Buzz”；

   3. 如果 *n* 同时是3和5的倍数，输出 “FizzBuzz”。

### 解题思路：

从1到n，首先判断n%3==0&&n%5==0，然后判断n%3==0，最后判断n%5==0。

```swift
class Solution {
    func fizzBuzz(_ n: Int) -> [String] {
        var result: [String] = []
        for i in 1...n  {
            if i%3 == 0, i%5 == 0 {
                result.append("FizzBuzz")
            } else if i%3 == 0 {
                result.append("Fizz")
            } else if i%5 == 0 {
                result.append("Buzz")
            } else {
                result.append("\(i)")
            }
        }
        return result
    }
}
```

### 算法升级：

判断是否能被3和5整除，用了两个判断条件，其实，可以使用3和5的最小公倍数来判断是否能被3和5整除。

```swift
class Solution {
    func fizzBuzz(_ n: Int) -> [String] {
        var result: [String] = []
        for i in 1...n  {
            if i%15 == 0 {
                result.append("FizzBuzz")
            } else if i%3 == 0 {
                result.append("Fizz")
            } else if i%5 == 0 {
                result.append("Buzz")
            } else {
                result.append("\(i)")
            }
        }
        return result
    }
}

```

### 另外的解法：

通过自己计算间隔是否等于3或者5来判断添加“Fizz”或者"Buzz"还是当前的下标。

```swift
class Solution {
    func fizzBuzz(_ n: Int) -> [String] {
        var result: [String] = []
        var fizzCheck = 0
        var buzzCheck = 0
        for i in 1...n  {
            fizzCheck = fizzCheck+1
            buzzCheck = buzzCheck+1
            var tmp = ""
            if fizzCheck == 3 {
                fizzCheck = 0
                tmp += "Fizz"
            }
            if buzzCheck == 5 {
                buzzCheck = 0
                tmp += "Buzz"
            }
            if tmp.isEmpty {
                result.append("\(i)")
            } else {
                result.append(tmp)
            }
        }
        return result
    }
}
```
