### 题目：

一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数。

**示例:**

```
输入: 19
输出:  true
解释: 12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

### 解题思路一：

因为如果是“快乐数”，则最终会变成1；如果不是“快乐数”，则最终也会变成个位数（如果所有位数加起来小于等于3就会变成个位数）。经计算证明，0～9里面只有1和7才符合“快乐数”的定义，所以，最终变成个位数时判断是否等于1或者7，否则不是“快乐数”。

```swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        guard n > 0 else {
            return false
        }

        let result = powAllNumber(n, n)
        return result == 1 || result == 7 ? true : false
    }

    func powAllNumber(_ n: Int, _ initialNumber: Int) -> Int {
        var sum = 0
        var res = n
        while res > 0 {
            let temp = res%10
            sum = sum + temp*temp
            res = res/10
        }
        if sum <= 9 {
            return sum
        } else {
            return powAllNumber(sum, initialNumber)
        }
    }
}
```

##### 算法简化：

在计算0～9是否为“快乐数”时，发现，所有的不快乐数都会进入 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 的循环中，所以算法可以这样实现。

```swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var m = n
        var sum = 0, d = 0
        while m > 0 {
            d = (m % 10)
            sum += d * d
            m = m / 10
        }
        if sum == 1 {
            return true
        }else if sum == 4 {
            return false
        }else {
            return isHappy(sum)
        }
    }
}

```

### 解题思路二：

判断是否进入循环，如果是则不是“快乐数”。

```swift
class Solution {
    func isHappy(_ n: Int) -> Bool {
        var visited = Set<Int>()
        var num = n
        while num != 1 && !visited.contains(num) {
            visited.insert(num)
            var sum = 0
            while num > 0 {
                let tmp = num%10
                num = num/10
                sum = sum + tmp*tmp
            }
            num = sum
        }
        return num == 1 ? true : false
    }
}
```
