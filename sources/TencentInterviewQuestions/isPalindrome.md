### 题目：

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```
示例 1:

输入: 121
输出: true
示例 2:

输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:

输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
进阶:
```

你能不将整数转为字符串来解决这个问题吗？

### 解法一：

将数字转换成字符串，然后再判断是否为回文数。

```swift
 class Solution {
    func isPalindrome(_ x: Int) -> Bool {
        if x < 0 || (x % 10 == 0 && x != 0) {
            return false
        }
        let strs = Array("\(x)")
        var i = 0
        var j = strs.count - 1
        while i < j {
            if strs[i] != strs[j] {
                return false
            }
            i += 1
            j -= 1
        }
        return true
    }
 }
```

### 解法二：

使用最高位和最低位比较法。

```swift
 class Solution {
    func isPalindrome(_ x: Int) -> Bool {
        if x < 0 || (x % 10 == 0 && x != 0) {
            return false
        }
        var num = x
        var div = 1
        while num / div >= 10 {
            div *= 10
        }
        while num > 0 {
            if num / div != num % 10 {
                return false
            }
            num = (num % div) / 10
            div /= 100
        }
        return true
    }
 }
```

### 解法三：

数字一半翻转比较法。

```swift
 class Solution {
    func isPalindrome(_ x: Int) -> Bool {
        if x < 0 || (x % 10 == 0 && x != 0) {
            return false
        }
        var revertedNumber = 0
        var num = x
        while num > revertedNumber {
            revertedNumber = revertedNumber * 10 + num % 10
            num /= 10
        }
        return num == revertedNumber || num == revertedNumber/10
    }
 }
```
