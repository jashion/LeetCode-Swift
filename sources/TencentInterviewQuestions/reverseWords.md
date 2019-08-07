### 题目：

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

```
示例 1:

输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。
```

### 解法一：

先以空格分割，然后再把分割的每一段字符串翻转，然后再用空格符连接。

```swift
 class Solution {
    func reverseWords(_ s: String) -> String {
        var res = s.split(separator: " ").map{String($0.reversed())}.reduce("") { (res, str) -> String in
            res+str+" "
        }
        return res.trimmingCharacters(in: NSCharacterSet.whitespaces)
    }
 }
```

### 解法二：

先整个字符串翻转，然后再以空格分割，然后以空格符倒序连接。

```swift
 class Solution {
    func reverseWords(_ s: String) -> String {
        var res = String(s.reversed()).split(separator: " ")
        var i = 0
        var j = res.count-1
        while i < j {
            res.swapAt(i, j)
            i += 1
            j -= 1
        }
        return res.reduce("", { (result, str) -> String in
            result + " " + str
        }).trimmingCharacters(in: NSCharacterSet.whitespaces)
    }
 }
```

### 解法三：

直接一遍遍历字符串，然后将每个单词翻转。

```swift
 class Solution {
    func reverseWords(_ s: String) -> String {
        var res = Array(s)
        var i = 0
        var j = 0
        var next = 0
        while j < res.count {
            if j == res.count-1 || res[j+1] == " " {
                next = j + 2
                while i < j {
                    res.swapAt(i, j)
                    i += 1
                    j -= 1
                }
                i = next
                j = next
            } else {
                j += 1
            }
        }
        return String(res)
    }
 }
```
