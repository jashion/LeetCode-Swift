### 题目：

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

案例:

```
s = "leetcode"
返回 0.

s = "loveleetcode",
返回 2.
```

注意事项：您可以假定该字符串只包含小写字母。

### 解法一：

遍历字符串，如果Set没有包含当前字符则添加，并且将当前字符以及对应的下标存入res，如果Set包含当前字符，则判断res是否存在当前字符，存在则删除，最后遍历完字符串res里面包含的都是没有重复字符的值和对应的下标，然后排序，返回第一个值的下标。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var set = Set<Character>()
        var res: [Character:Int] = [:]
        for (i, value) in s.enumerated() {
            print(res)
            if !set.contains(value) {
                set.insert(value)
                res[value] = i
            } else {
                if res.keys.contains(value) {
                    res.removeValue(forKey: value)
                }
            }
        }
        if res.isEmpty {
            return -1
        } else {
            return res.sorted(by: {$1.value>$0.value}).first!.value
        }
    }
 }
```

解决的思路差不多，不过res只存不重复的字符，最后通过字符查找下标返回。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var set = Set<Character>()
        var res: [Character] = []
        for value in s {
            if !set.contains(value) {
                set.insert(value)
                res.append(value)
            } else {
                if res.contains(value) {
                    res.remove(at: res.firstIndex(of: value)!)
                }
            }
        }
        if res.isEmpty {
            return -1
        } else {
            return s.distance(from: s.startIndex, to: s.firstIndex(of: res[0])!)
        }
    }
 }
```

### 解法二：

遍历字符串，然后将字符作为key，下标和字符出现的次数的元组作为value存入字典中。最后，遍历字典，找出字符出现次数=1的字符，然后取下标最小的字符。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var dict: [Character: (Int, Int)] = [:] //(index, count)
        for (i, c) in s.enumerated() {
            if let tmp = dict[c] {
                dict[c] = (tmp.0, tmp.1+1)
            } else {
                dict[c] = (i, 1)
            }
        }
        var res = -1
        for (i, c) in dict.values {
            if c == 1 {
                if res == -1 {
                    res = i
                } else {
                    res = min(res, i)
                }
            }
        }
        return res
    }
 }
```

### 解法三：

遍历字符串，把字符作为key，然后重复出现的字符的value=-1，唯一的字符则value=index，然后过滤掉value=-1的值，然后再以value排序，返回第一个值的value。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var dict: [Character: Int] = [:]
        for (i, c) in s.enumerated() {
            if let _ = dict[c] {
                dict[c] = -1
            } else {
                dict[c] = i
            }
        }
        let tmp = dict.filter{$1 != -1}.sorted(by: {$0.value < $1.value})
        if tmp.count > 0 {
            return tmp.first!.value
        } else {
            return -1
        }
    }
 }
```

思路一样，代码更简洁。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var dict: [Character: Int] = [:]
        for c in s {
            dict[c] = (dict[c] ?? 0) + 1
        }
        for (i, c) in s.enumerated() {
            if dict[c] == 1 {
                return i
            }
        }
        return -1
    }
 }
```

### 解法四：

字母法。因为字符串包含的都是小写字母，也就是只有26位，对应a~z，遍历字符串，将每个字符出现的次数存在相应的数组下标里。然后再次遍历字符串，找出第一个值为1的字符，返回当前的下标。

```swift
 class Solution {
    func firstUniqChar(_ s: String) -> Int {
        var arr = Array.init(repeating: 0, count: 26)
        for c in s.unicodeScalars {
            arr[Int(c.value)-97] += 1
        }
        for (i, c) in s.unicodeScalars.enumerated() {
            if arr[Int(c.value)-97] == 1 {
                return i
            }
        }
        return -1
    }
 }
```
