### 题目：

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。

示例 1:

```
输入: s = "anagram", t = "nagaram"
输出: true
```
示例 2:

```
输入: s = "rat", t = "car"
输出: false
```
说明:
你可以假设字符串只包含小写字母。

进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

### 解法一：

将字符串转换成字符串数组，然后对比。

```swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        guard s.count == t.count else {
            return false
        }

        var sChars = Array(s)
        var tChars = Array(t)
        for i in 0..<sChars.count {
            if let index = tChars.firstIndex(of: sChars[i]) {
                tChars.remove(at: index)
            } else {
                return false
            }
        }
        return true
    }
}
```

### 解法二：

字符串直接对比。

```swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        guard s.count == t.count else {
            return false
        }
        
        var tmp = t
        for value in s {
            if let index = tmp.firstIndex(of: value) {
                tmp.remove(at: index)
            }
        }
        if tmp.isEmpty {
            return true
        }
        return false
    }
}
```

### 解法三：

使用26个字母，时间复杂度为O(n)。

```swift
class Solution {
    func isAnagram(_ s: String, _ t: String) -> Bool {
        guard s.count == t.count else {
            return false
        }
        
        var total = Array(repeating: 0, count: 26)
        let a = "a".unicodeScalars.first!.value
        for c in s.unicodeScalars {
            let index = Int(c.value-a)
            total[index] = total[index] + 1
        }
        for c in t.unicodeScalars {
            let index = Int(c.value-a)
            if total[index] <= 0 {
                return false
            }
            total[index] = total[index]-1
        }
        return true
    }
}
```


