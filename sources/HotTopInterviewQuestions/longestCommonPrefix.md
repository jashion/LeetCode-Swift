### 题目：

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```
示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。
```

### 解法一：

水平扫描法，获取每个字符串的第一个字符比较，如果都一样，则继续第二个字符比较，如果有一个不一样，则返回。

```swift
 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        var prefix: String = ""
        guard !strs.isEmpty else {
            return prefix
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        let lenght: Int = strs.reduce(Int.max, {x, y in
            min(x, y.count)
        })

        for offset in 0..<lenght {
            for (i, value) in strs.enumerated() {
                let index = value.index(value.startIndex, offsetBy: offset)
                let temp = value[index]
                if i == 0 {
                    prefix.append(temp)
                } else {
                    if prefix[index] != temp {
                        prefix.remove(at: index)
                        return prefix
                    }
                }
            }
        }
        return prefix
    }
 }
```

### 解法二：

逐个比较法，先获取第一和第二个字符串的前缀，如果没有公共前缀，则直接返回，如果有，则继续获取该前缀和第三个字符串的公共前缀，直到遍历完整个字符串数组或者没有公共前缀，则返回。

```swift
 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else {
            return ""
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        var res: String = ""
        for item in strs {
            if item == "" {
                return ""
            } else if res == "" {
                res = item
            } else {
                if res == item {
                    continue
                }
                let str1 = item.count < res.count ? item : res
                let str2 = item.count >= res.count ? item : res

                var offset = 0
                for i in 0..<str1.count {
                    if str1[str1.index(str1.startIndex, offsetBy: i)] !=  str2[str2.index(str2.startIndex, offsetBy: i)]{
                        break
                    } else {
                        offset += 1
                    }
                }
                if offset == 0 {
                    return ""
                } else {
                    res = String(str1.prefix(offset))
                }
            }
        }
        return res
    }
 }
```

### 解法三：

最短字符串比较法，获取字符串数组里长度最短的字符串，以该字符串匹配字符串数组里所有的字符串。如果匹配的字符串的前缀包含最短字符串，则继续匹配下一个，如果不包含，则最短字符串移除最后一个字符，继续判断匹配的字符串的前缀是否包含最短字符串，直到最短字符串为空或者遍历完整个数组，返回。

```swift
 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else {
            return ""
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        var shortStr = strs.reduce(strs[0], {x, y in
            x.count < y.count ? x : y
        })
        for str in strs {
            while !shortStr.isEmpty {
                if str.hasPrefix(shortStr) {
                    break
                } else {
                    shortStr.removeLast()
                }
            }
        }
        return shortStr
    }
 }
```

### 解法四：

分治法，将需要解决的大问题分解为小问题。

```swift
 //分治法
 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else {
            return ""
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        return getLongestCommonPrefix(strs, 0, strs.count-1)
    }

    func getLongestCommonPrefix(_ strs: [String], _ left: Int, _ right: Int) -> String {
        if left == right {
            return strs[left]
        } else {
            let mid = (left + right) / 2
            let lcpLeft = getLongestCommonPrefix(strs, left, mid)
            let lcpRight = getLongestCommonPrefix(strs, mid+1, right)
            return commonPrefix(lcpLeft, lcpRight)
        }
    }

    func commonPrefix(_ left: String, _ right: String) -> String {
        let length = min(left.count, right.count)
        for i in 0..<length {
            if left[left.index(left.startIndex, offsetBy: i)] != right[right.index(right.startIndex, offsetBy: i)] {
                return String(left.prefix(i))
            }
        }
        return String(left.prefix(length))
    }
 }
```

### 解法五：

二分查找法，获取长度最短的字符串str，然后从str[0..mid]开始比较所有字符串的前缀是否包含，该字符串，如果包含继续比较str[0...(mid+1)]，如果不包含继续比较str[0...(mid-1)]，直到str的长度为0或者长度等于最短字符串，返回结果。

```swift
 //二分查找法
 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else {
            return ""
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        var minLen = Int.max
        minLen = strs.reduce(minLen, {x, y in
            min(x, y.count)
        })
        var low =  1
        var high = minLen
        while low <= high {
            let middle = (low + high) / 2
            if commonPrefix(strs, middle) {
                low = middle + 1
            } else {
                high = middle - 1
            }
        }
        return String(strs.first!.prefix((low+high)/2))
    }


    func commonPrefix(_ strs: [String], _ length: Int) -> Bool {
        let str = String(strs.first!.prefix(length))
        var i = 1
        while i < strs.count {
            if !strs[i].hasPrefix(str) {
                return false
            }
            i += 1
        }
        return true
    }
 }
```

### 解法六：

[字典树]([https://leetcode-cn.com/problems/implement-trie-prefix-tree/](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)。

```swift
 //字典树
 class TrieNode {
    var child: [TrieNode?] = Array.init(repeating: nil, count: 26)
    var isEnd: Bool = false
    var size = 0

    func put(_ ch: Character) -> TrieNode {
        let i = Int(ch.unicodeScalars.first!.value) - Int("a".unicodeScalars.first!.value)
        if child[i] == nil {
            child[i] = TrieNode()
            size += 1
        }
        return child[i]!
    }
 }

 class Trie {
    var root: TrieNode?

    /** Initialize your data structure here. */
    init() {
        root = TrieNode()
    }

    /** Inserts a word into the trie. */
    func insert(_ word: String) {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in word {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                node?.put(c)
            }
            node = node?.child[i]
        }
        node?.isEnd = true
    }

    /** Returns if the word is in the trie. */
    func search(_ word: String) -> Bool {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in word {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                return false
            }
            node = node?.child[i]
        }
        return node!.isEnd
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    func startsWith(_ prefix: String) -> Bool {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in prefix {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                return false
            }
            node = node?.child[i]
        }
        return true
    }

    func searchLongestPrefix(_ word: String) -> String {
        var node = root
        var res: String = ""
        for c in word {
            let i = Int(c.unicodeScalars.first!.value) - Int("a".unicodeScalars.first!.value)
            if node!.child[i] != nil, node!.size == 1, !node!.isEnd{
                res.append(c)
                node = node?.child[i]
            } else {
                return res
            }
        }
        return res
    }
 }


 class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        guard !strs.isEmpty else {
            return ""
        }
        guard strs.count > 1 else {
            return strs.first!
        }
        if strs.first! == "" {
            return ""
        }

        let trie = Trie()
        for s in strs {
            trie.insert(s)
        }
        return trie.searchLongestPrefix(strs[0])
    }
}
```
