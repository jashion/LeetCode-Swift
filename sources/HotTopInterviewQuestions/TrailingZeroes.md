### 题目：

给定一个整数 n，返回 n! 结果尾数中零的数量。

示例 1:

```
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
```
示例 2:

```
输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
```
说明: 你算法的时间复杂度应为 O(log n) 。

### 解法一：

暴力法，计算n!的结果再统计尾数中的零。但是，如果n很大的话，时间复杂度太高，并且可能计算的时候越界，所以，可以Pass掉。

### 解法二：

寻找一般规律，10!=1x2x3x4x5x6x7x8x9x10=3628800，其中有两个零，注意到10，5x2=10，也就是末尾的两个0，所以只要寻找到一对2和5，就可以确定一个0。因为n!中的偶数和奇数各一半，偶数都可以分成成2X的方式，而其中5或者5的倍数就相对2来说比较少，所以，只要找到5的倍数就可以确定末尾0的个数了。比如：n=100，那么其中有多少个5呢？num5=100/5=20，可以看作从1开始，每间隔4为一个周期，也就是5，10，15，20...100，继续往下，5^2=25，一个5算一个0，5^2=5x5，可以算两个0，因为5^2也是5的倍数，之前计算5也算了一次，可以寻找25的时候，在之前的基础上加一就行了，那么有多少个25呢？100/25=100/5/5=4，也就是4个25，所以总的0的个数count=100/5+100/5/5=24。可以得出结论，n!末尾的0为n包含5，25，125...的总个数。

```swift
 func trailingZeroes(_ n: Int) -> Int {
    var count = 0
    var num = n
    while num >= 5 {
        count += num/5
        num /= 5
    }
    return count
 }
```

### 解法三：

也是基于5的基础上计算的，但是统计的比较复杂。

```swift
  func trailingZeroes(_ n: Int) -> Int {
    guard n >= 5 else {
        return 0
    }
    var num = 0
    var i = 5
    while i <= n {
        if i%5 == 0 {
            var tmp = i
            while tmp%5 == 0 {
                num += 1
                tmp = tmp/5
            }
        }
        i += 5
    }
    return num
 }
```

### 解法四：

在解法三的基础上，添加了缓存，提高效率。

```swift
 func trailingZeroes(_ n: Int) -> Int {
    if n < 5 {
        return 0
    }
    if n < 10 {
        return 1
    }
    var num = 1
    var i = 10
    var dict: [Int:Int] = [5:1]
    while i <= n {
        if i%5 == 0 {
            if let zeros = dict[i] {
                num += zeros
            } else {
                var tmp = i
                var zeros = 0
                while tmp%5 == 0 {
                    zeros += 1
                    tmp = tmp/5
                    if let zeros2 = dict[tmp] {
                        zeros += zeros2
                        break
                    }
                }
                num += zeros
                dict[i] = zeros
            }
        }
        i += 5
    }
    return num
 }
```
