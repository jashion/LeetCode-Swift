### 题目：

**不使用**运算符 `+` 和 `-` ​​​​​​​，计算两整数 ​​​​​​​`a` 、`b` ​​​​​​​之和。

**示例 1:**

```
输入: a = 1, b = 2
输出: 3
```

**示例 2:**

```
输入: a = -2, b = 3
输出: 1
```

### 解题思路：

因为不能直接用加减符号，那么就需要使用其他的操作符来代替加减。计算机上的加减归根结底都是二进制的加法，那么二进制相加可以使用异或（^）来进行，但是需要解决相加进位的问题，因为只有1+1才会进位，那么可以使用与（&）来计算进位，因为进位需要到更高一位加1，所需还需要往左移动一位，那么，实现的算法如下：

```swift
class Solution {
    func getSum(_ a: Int, _ b: Int) -> Int {
        var sum = 0, carry = 0
        sum = a ^ b
        carry = (a & b) << 1
        if carry != 0 {
            return getSum(sum, carry)
        }
        return sum
    }
}

//更简洁的实现
class Solution {
    func getSum(_ a: Int, _ b: Int) -> Int {
        return b == 0 ? a : getSum(a ^ b, (a & b)<<1)
    }
}
```
