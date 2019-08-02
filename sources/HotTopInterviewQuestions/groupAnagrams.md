### 题目：

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

```
示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
["ate","eat","tea"],
["nat","tan"],
["bat"]
]
说明：

所有输入均为小写字母。
不考虑答案输出的顺序。
```

### 解法一：

暴力法一，将每个字符串都变成一个字符字典，以字符的编码为key，统计每个字符出现的次数，然后逐一比较。(超过时间限制)

```swift
 class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        let start = Int("a".unicodeScalars.first!.value)
        var strArray = strs
        var match = [[Int:Int]]()
        for str in strs {
            var tmp = [Int:Int]()
            for ch in str.unicodeScalars {
                let i = Int(ch.value)-start
                if tmp[i] != nil {
                    tmp[i] = tmp[i]! + 1
                } else {
                    tmp[i] = 1
                }
            }
            match.append(tmp)
        }
        var res = [[String]]()
        while !match.isEmpty {
            let item = match.removeFirst()
            var tmp = [String]()
            tmp.append(strArray.removeFirst())
            var j = 0
            while j < match.count {
                var matchStr = match[j]
                if item.count != matchStr.count {
                    j += 1
                    continue
                }
                var hasMatch = true
                for dic in item {
                    if matchStr[dic.key] != dic.value{
                        hasMatch = false
                        break
                    }
                }
                if hasMatch {
                    tmp.append(strArray.remove(at: j))
                    match.remove(at: j)
                } else {
                    j += 1
                }
            }
            res.append(tmp)
        }
        return res
    }
 }
```

### 解法二：

暴力法二，优化。直接字符串排序，再逐一比较。（ 超过时间限制）

```swift
 class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var tmp =  strs
        var res = [[String]]()
        while !tmp.isEmpty {
            var match = [String]()
            let item = tmp.removeFirst()
            match.append(item)
            var i = 0
            while i < tmp.count {
                if item.sorted(by: <).elementsEqual(tmp[i].sorted(by: <)) {
                    match.append(tmp.remove(at: i))
                } else {
                    i += 1
                }
            }
            res.append(match)
        }
        return res
    }
 }

```

### 解法三：

暴力法三，字符串数组法。因为只是26个小写字母，可以构建一个26长度的数组，统计每个字母出现的次数。（超出时间限制）

```swift
 class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var tmp =  strs
        var res = [[String]]()
        let start = Int("a".unicodeScalars.first!.value)
        while !tmp.isEmpty {
            var match = [String]()
            let item = tmp.removeFirst()
            match.append(item)
            var i = 0
            while i < tmp.count {
                var arr = Array.init(repeating: 0, count: 26)
                for ch in item.unicodeScalars {
                    let index = Int(ch.value)-start
                    arr[index] += 1
                }
                var arr2 = Array.init(repeating: 0, count: 26)
                for ch in tmp[i].unicodeScalars {
                    let index = Int(ch.value)-start
                    arr2[index] += 1
                }
                if arr.elementsEqual(arr2) {
                    match.append(tmp.remove(at: i))
                } else {
                    i += 1
                }
            }
            res.append(match)
        }
        return res
    }
 }
```

### 解法四：

也是字母数组法，不过，使用了字典，效率大大提高了。因为，如果是字母异位词，字母数组是相等的，所以，可以直接用字母数组做为key来统计字母异位词。

```swift
 class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var res = [[Character]:[String]]()
        for str in strs {
            let ch = Array(str).sorted()
            if res[ch] == nil {
                res[ch] = [str]
            } else {
                res[ch]?.append(str)
            }
        }
        return Array(res.values)
    }
 }

```

### 解法五：

字母字符串法，和解法四思路一样，只不过字母数组改成了了字符串。

```swift
 //按计数分类
 class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var res = [String:[String]]()
        let start = Int("a".unicodeScalars.first!.value)
        for str in strs {
            var count = Array.init(repeating: 0, count: 26)
            var keyStr = ""
            for ch in str.unicodeScalars {
                let index = Int(ch.value)-start
                count[index] += 1
            }
            for item in count {
                keyStr.append("#\(item)")
            }
            if res[keyStr] == nil {
                res[keyStr] = [str]
            } else {
                res[keyStr]?.append(str)
            }
        }
        return Array(res.values)
    }
 }
```
