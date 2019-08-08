### 题目：

给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

```
示例 1:

输入: 1
输出: true
解释: 20 = 1
示例 2:

输入: 16
输出: true
解释: 24 = 16
示例 3:

输入: 218
输出: false
```

### 解法一：

除二取余法。

```swift
 class Solution {
    func isPowerOfTwo(_ n: Int) -> Bool {
        if n <= 0 {
            return false
        }
        var num = n
        while num % 2 == 0 {
            num = num / 2
        }
        return num == 1
    }
 }
```

### 解法二：

二进制法，因为2的倍数的二进制都是最高位为1，其余位都为0，并且，如果该元素是2的倍数，那么该元素-1正好是所有位都位1的二进制，所以n&(n-1)=0就是2的倍数。

```swift
 class Solution {
    func isPowerOfTwo(_ n: Int) -> Bool {
        return n > 0 && (n & (n-1) == 0)
    }
 }
```
