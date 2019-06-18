### 题目：

给定一个整数，写一个函数来判断它是否是 3 的幂次方。

```
示例 1:

输入: 27
输出: true
示例 2:

输入: 0
输出: false
示例 3:

输入: 9
输出: true
示例 4:

输入: 45
输出: false
```

进阶：
你能不使用循环或者递归来完成本题吗？

### 解法一：递归

不断判断是否能被3整除。

```swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        if n != 0, n%3 == 0 {
            return isPowerOfThree(n/3)
        }

        if n == 1 {
            return true
        }
        return false
    }
}
```



### 解法二：迭代除法

和解法一思路一样，也是不断判断是否能被3整除。

```swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        var res = n
        while res != 0, res%3 == 0 {
            res = res/3
        }

        if res == 1 {
            return true
        } else {
            return false
        }
    }
}
```

### 解法三：迭代乘法

前面的两个解题思路都是除法，现在改成乘法。res从1开始，不断的乘以3，直到res>=n。如果res=n，则是3的幂次方，否则不是。

```swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        var res = 1
        var i = 1
        while res < n, n != res {
            res = Int(pow(Double(3), Double(i)))
            i += 1
        }

        if n == res  {
            return true
        } else {
            return false
        }
    }
}
```

### 解法四：最大值法

找到整形中3的最大次幂，Int32 = 3^19 = 1162261467 Int64 = 3^39 = 4052555153018976256。那么，任何3的次幂都是少于或者等于它，并且n可以被它整除，那么说明n是3的次幂，否则不是。

```swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        return n>0 && 4052555153018976256%n == 0
    }
}
```

### 解法五：公式法

如果n是3的次幂，那么对n取对数可以得到一个整数：log10(n)/log10(3)，如果不是整数，则不是3的次幂。但是对于n=243, 59049...本来应该是返回整数，但是返回的是无限接近的整数，所以，使用该方法需要谨慎。

```swift
class Solution {
    func isPowerOfThree(_ n: Int) -> Bool {
        if n <= 0 {
            return false
        }
        let i: Double = log(Double(n))/log(3.0)
        return i == Double(Int(i))
    }
}
```
