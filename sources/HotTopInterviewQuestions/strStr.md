### 题目：

实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

```
示例 1:

输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:

输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

说明:

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。

### 解法一：

暴力法。

```swift
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        guard haystack != needle else {
            return 0
        }

        let hArray = Array(haystack)
        let nArray = Array(needle)
        for i in 0..<hArray.count {
            if i + nArray.count > hArray.count {
                break;
            }
            var hc = hArray[i]
            var isOk = true
            for j in 0..<nArray.count {
                let nc = nArray[j]
                if hc != nc {
                    isOk = false
                    break
                } else if i + j + 1 < hArray.count{
                    hc = hArray[i + j + 1]
                } else {
                    break;
                }
            }
            if isOk {
                return i
            }
        }
        return -1
    }
 }
```

### 解法二：

使用Swift语言封装的字符串匹配函数。

```swift
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        if let index = haystack.range(of: needle) {
            return index.lowerBound.utf16Offset(in: haystack)
        }
        return -1
    }
 }
```

### 解法三：

下标法。

```swift
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        guard haystack != needle else {
            return 0
        }
        for i in 0...haystack.count-needle.count {
            if haystack[haystack.index(haystack.startIndex, offsetBy: i)...haystack.index(haystack.startIndex, offsetBy: i+needle.count)] == needle {
                return i
            }
        }
        return -1
    }
 }
```

### 解法四：

下标法二。

```swift
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        guard haystack != needle else {
            return 0
        }
        let hStr = haystack.map{String($0)}
        let nStr = needle.map{String($0)}
        for i in 0...(hStr.count-nStr.count) {
            print("i: \(i)")
            for j in 0..<nStr.count {
                print("i: \(i) j: \(j)")
                if hStr[i+j] != nStr[j] {
                    break
                }
                if j == nStr.count-1 {
                    return i
                }
            }
        }
        return -1
    }
 }
```

### 解法五：

前缀后缀法。

```swift

 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        guard haystack != needle else {
            return 0
        }
        var temp = haystack
        var index = -1
        while temp.count >= needle.count {
            index += 1
            if temp.hasPrefix(needle) {
                return index
            }
            temp = String(temp.suffix(temp.count-1))
        }
        return -1
    }
 }
```

### 解法六：

[KMP算法]([https://www.jianshu.com/p/68e9a227cb45](https://www.jianshu.com/p/68e9a227cb45)。

```swift
 //KMP
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }

        let next = getNext(needle)
        var i = 0
        var j = 0
        let iStart = haystack.startIndex
        let jStart = needle.startIndex
        while i < haystack.count, j < needle.count {
            if j == -1 || haystack[haystack.index(iStart, offsetBy: i)] == needle[needle.index(jStart, offsetBy: j)] {
                i += 1
                j += 1
            } else {
                j = next[j]
            }
        }

        if j == needle.count {
            return i-needle.count
        } else {
            return -1
        }
    }

    func getNext(_ str: String) -> [Int] {
        guard !str.isEmpty else {
            return []
        }

        var next: [Int] = [-1]
        var i = 0
        var k = -1
        let startIndex = str.startIndex
        while i < str.count-1 {
            if k == -1 || str[str.index(startIndex, offsetBy: i)] == str[str.index(startIndex, offsetBy: k)] {
                k += 1
                i += 1
                next.append(k)
            } else {
                k = next[k]
            }
        }
        return next
    }
 }
```

### 解法七：

KMP改进算法算法。

```swift
 //KMP next数组优化
 class Solution {
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }

        let next = getNext(needle)
        var i = 0
        var j = 0
        let iStart = haystack.startIndex
        let jStart = needle.startIndex
        while i < haystack.count, j < needle.count {
            if j == -1 || haystack[haystack.index(iStart, offsetBy: i)] == needle[needle.index(jStart, offsetBy: j)] {
                i += 1
                j += 1
            } else {
                j = next[j]
            }
        }

        if j == needle.count {
            return i-needle.count
        } else {
            return -1
        }
    }

    func getNext(_ str: String) -> [Int] {
        guard !str.isEmpty else {
            return []
        }

        var next: [Int] = [-1]
        var i = 0
        var k = -1
        let startIndex = str.startIndex
        while i < str.count-1 {
            if k == -1 || str[str.index(startIndex, offsetBy: i)] == str[str.index(startIndex, offsetBy: k)] {
                k += 1
                i += 1
                if str[str.index(startIndex, offsetBy: i)] != str[str.index(startIndex, offsetBy: k)] {
                    next.append(k)
                } else {
                    next.append(next[k])
                }
            } else {
                k = next[k]
            }
        }
        return next
    }
 }
```

### 解法八：

[Sunday算法]([https://blog.csdn.net/qq_33515733/article/details/81163135](https://blog.csdn.net/qq_33515733/article/details/81163135)。

```swift
 //Sunday算法
 class Solution {
    func sundayPre(_ str: String) -> [Int] {
        var res = Array.init(repeating: str.count+1, count: 256)
        for (i, c) in str.unicodeScalars.enumerated() {
            res[Int(c.value)] = str.count-i
        }
        return res
    }

    func strStr(_ haystack: String, _ needle: String) -> Int {
        var post = 0
        var j = 0
        let l1 = haystack.count
        let l2 = needle.count
        let start1 = haystack.startIndex
        let start2 = needle.startIndex
        let shift = sundayPre(needle)

        while post <= l1-l2 {
            j = 0
            while j < l2, haystack[haystack.index(start1, offsetBy: post+j)] == needle[needle.index(start2, offsetBy: j)] {
                j += 1
            }
            if j >= l2 {
                return post
            } else {
                if post < l1-l2 {
                    post += shift[Int(haystack[haystack.index(start1, offsetBy: post+l2)].unicodeScalars.first!.value)]
                } else {
                    return -1
                }
            }
        }
        return -1
    }
 }
```

### 解法九：

BM算法。

[参考一]([http://www-igm.univ-mlv.fr/~lecroq/string/node14.html](http://www-igm.univ-mlv.fr/~lecroq/string/node14.html)

[参考二]([https://blog.csdn.net/chenhanzhun/article/details/39938989](https://blog.csdn.net/chenhanzhun/article/details/39938989)

[参考三]([http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html](http://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html)

```swift
 //BM
 class Solution {
    //最坏字符串匹数组：假如字符x在字符串s某处不匹配，则找到x在s的最右边位置i(i < m - 1)，然后x+1和i+1开始匹配，如果x不存在s，则直接x+1和s的第一个字符开始匹配
    //m为需要匹配数组的长度，Bc[m-1]=m，因为B[m]不存在，移动没有意义，所以从头开始
    func preBmBc(_ str: String) -> [Int] {
        let m = str.count
        let sArray = Array(str)
        var bmBc = Array.init(repeating: m, count: 256) //一个字符一个字节(8bit=2^8=256个字符)表示
        for i in 0..<m - 1 {
            bmBc[Int(sArray[i].unicodeScalars.first!.value)] = m - 1 - i
        }
        print("bmBc: \(bmBc)")
        return bmBc
    }
    
    //好字符串辅助数组
    //suff[i]=s，表示以i为边界，与模式串后缀匹配的最大长度
    func suffixes(_ str: String) -> [Int] {
        let m = str.count
        let array = Array(str)
        var suff = Array.init(repeating: 0, count: m)
        suff[m-1] = m //suff[m-1]=m，因为最后一个位置不匹配，则前缀和后缀都是整个字符串
        var i = m - 2
        var j = 0
        while i >= 0 {
            j = i
            //从s[m-1]，s[m-2]往回开始比较
            while j >= 0, array[j] == array[m-1-(i-j)] {
                j -= 1
            }
            suff[i] = i - j
            i -= 1
        }
        return suff
    }
    
    //最好字符串数组：假如s和t尾部部分匹配，则找到匹配字符串的最右位置，重新匹配
    //1.假如字符串t没有匹配字符串，则整个移动
    //2.假如字符串t前缀部分匹配后缀，则移动到部分匹配的位置
    //3.假如字符串t有多个匹配字符串，找最右一个
    func preBmGs(_ str: String) -> [Int] {
        let suff = suffixes(str) //辅助数组
        print("suff: \(suff)")
        let m = str.count
        
        //第一种情况，没有好后缀
        var bmGs = Array.init(repeating: m, count: m)
        print(bmGs)
        
        //第二种情况，模式串存在与好后缀部分匹配的子串
        var i = m-1
        var j = 0
        while i >= 0 {
            //如果存在只会有一个，所以j可以不用管
            //比如：abcdefgabcd
            //很明显：前缀abcd和后缀abcd符合则假如在abcdefg任意一个不配，
            //则需要移动到后缀abcd，如果再后缀abcd失配，
            //则需要移动整个字符串，因为没有比abcd更小的部分匹配字符串
            if suff[i] == i + 1 {
                while j < m - 1 - i {
                    if bmGs[j] == m {
                        bmGs[j] = m - 1 - i
                    }
                    j += 1
                }
            }
            i -= 1
        }
        print(bmGs)
        
        //第三种情况，模式串存在与好后缀完全匹配的字串
        i = 0
        while i <= m - 2 {
            bmGs[m - 1 - suff[i]] = m - 1 - i
            i += 1
        }
        print(bmGs)
        
        return bmGs
    }
    
    func strStr(_ haystack: String, _ needle: String) -> Int {
        guard !needle.isEmpty else {
            return 0
        }
        guard haystack.count >= needle.count else {
            return -1
        }
        guard haystack != needle else {
            return 0
        }
        
        let hArray = Array(haystack)
        let nArray = Array(needle)
        let bmBc = preBmBc(needle)
        let bmGs = preBmGs(needle)
        let hLen = hArray.count
        let nLen = nArray.count
        
        var j = 0
        while j <= hLen - nLen {
            var i = nLen - 1
            while i >= 0, nArray[i] == hArray[i + j] {
                i -= 1
            }
            if i < 0 {
                return j
            } else {
                j += max(bmGs[i], bmBc[Int(hArray[i+j].unicodeScalars.first!.value)] - nLen + 1 + i)
            }
        }
        return -1
    }
 }
```
