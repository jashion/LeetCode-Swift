### 题目：

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

示例 2:

```
输入: "race a car"
输出: false
```

### 解法一：暴力法

先过滤掉非数字非字母的字符，并且将剩下的字符全部转成大写。然后，使用首尾指针，遍历判断是否是回文数。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        let str = s.filter{$0.isNumber||$0.isLetter}.uppercased()
        var i = 0
        var j = str.count-1
        let start = str.startIndex
        while i < j {
            let left = str.index(start, offsetBy: i)
            let right = str.index(start, offsetBy: j)
            if str[left] != str[right] {
                return false
            } else {
                i += 1
                j -= 1
            }
        }
        return true
    }
 }
```

### 解法二：暴力法二

和解法一思路一样，不过这里使用四个指针，分别是首尾和中部的两个指针，提高效率。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        let str = s.filter{$0.isNumber||$0.isLetter}.uppercased()
        if str.isEmpty || str.count == 1{
            return true
        }
        if str.count == 2 {
            if str[str.startIndex] == str[str.index(before: str.endIndex)] {
                return true
            } else {
                return false
            }
        }
        var i = 0
        var k = (str.count-1)/2
        var m = str.count%2 == 0 ? k+1 : k
        var j = str.count-1
        let start = str.startIndex
        while i < k, m < j {
            let left = str.index(start, offsetBy: i)
            let midLeft = str.index(start, offsetBy: k)
            let midRight = str.index(start, offsetBy: m)
            let right = str.index(start, offsetBy: j)
            if str[left] != str[right] || str[midLeft] != str[midRight] {
                return false
            } else {
                i += 1
                k -= 1
                m += 1
                j -= 1
            }
        }
        return true
    }
 }
```

### 解法三：暴力法三

和解法一的思路一样，多加了两个循环。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        if s.count <= 1 {
            return true
        }
        var i = 0
        var j = s.count-1
        let start = s.startIndex
        while i < j {
            var left = s.index(start, offsetBy: i)
            while i < j, !s[left].isNumber, !s[left].isLetter {
                i += 1
                left = s.index(start, offsetBy: i)
            }
            var right = s.index(start, offsetBy: j)
            while i < j, !s[right].isNumber, !s[right].isLetter{
                j -= 1
                right = s.index(start, offsetBy: j)
            }
            if s[left].uppercased() != s[right].uppercased() {
                return false
            } else {
                i += 1
                j -= 1
            }
        }
        return true
    }
 }
```

### 解法四：前后字符串比较法

将字符串前半段和字符串后半段比较，判断是否相等。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        if s.count <= 1 {
            return true
        }

        let str = s.filter{$0.isNumber||$0.isLetter}.lowercased()
        var leftStr: String
        var rightStr: String
        if str.count%2 == 0 {
            leftStr = String(str.prefix(str.count/2))
            rightStr = String(str.suffix(str.count/2))
        } else {
            leftStr = String(str.prefix(str.count/2+1))
            rightStr = String(str.suffix(str.count/2+1))
        }
        if leftStr == String(rightStr.reversed()) {
            return true
        } else {
            return false
        }
    }
 }
```

### 解法五：字符串比较法

和解法四思路有点像，这个是整个字符串比较。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        if s.count <= 1 {
            return true
        }

        let str = s.filter{$0.isNumber||$0.isLetter}.lowercased()
        return str == String(str.reversed()) ? true : false
    }
 }
```

### 解法六：数组字符法

将字符串分割成n个字符的数组，然后使用数组做比较。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        if s.count <= 1 {
            return true
        }

        var tmp: [String] = []
        for item in s {
            if item.isLetter || item.isNumber {
                tmp.append(item.uppercased())
            }
        }

        var i = 0
        var j = tmp.count-1
        while i < j {
            if tmp[i] != tmp[j] {
                return false
            } else {
                i += 1
                j -= 1
            }
        }
        return true
    }
 }
```

### 解法七：数组ASCII码法

将字符串分割成n个字符的ASCII码的数组，然后使用数组做比较。

```swift
 class Solution {
    func isPalindrome(_ s: String) -> Bool {
        if s.isEmpty {
            return true
        }
        var arr = Array<UInt8>()
        for item in s.utf8 {
            if item >= 97 && item <= 122 {
                arr.append(item)
            }
            if item >= 65 && item <= 90 {
                arr.append(item + 32)
            }
            if item >= 48 && item <= 57 {
                arr.append(item)
            }
        }

        var i = 0
        var j = arr.count - 1
        while i < j {
            if arr[i] == arr[j] {
                j -= 1
                i += 1
            } else {
                return false
            }
        }
        return true
    }
 }
```
