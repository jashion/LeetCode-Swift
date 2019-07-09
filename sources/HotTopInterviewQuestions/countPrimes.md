### 题目：

统计所有小于非负整数 *n* 的质数的数量。

**示例:**

```
输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

### 解法一：

暴力法，计算0～n-1是否为质数。

```swift
 //暴力法
 class Solution {
    func countPrimes(_ n: Int) -> Int {
        guard n > 2 else {
            return 0
        }

        var count = 0
        for i in 2..<n {
            if isPrime(i) {
                count += 1
                print("i: \(i)")
            }
        }
        return count
    }

    //n若不可以被2...√n整除，则说明n是质数
    //为什是2～√n？素数的定义：一个数只能被1和本身整除，被称作质数或者素数。
    //那么，反之如果一个数可以被1和本身之外的其他数整除，则被称作合数。
    //那么，证明一个数n不是质数，只需要证明n不能被2~n-1整除就可以了，但是效率比较低
    //假如n可以被2整除，那么就是将n等分也就是2*n/2，同理可以被3整除，3等分3*n/3...
    //也就是一个因子增大的同时，另一个因子肯定在减少，那么两个因子差距最小的是什么时候？也就是√n
    //可以看作一个因子从2～√n，另外一个因子肯定从√n~2
    //所以，只需要证明n没有被2~√n之间的数整除，就可以得出n是否是质数了
    func isPrime(_ n: Int) -> Bool {
        if n <= 1 {
            return false
        }
        if n == 2 {
            return true
        }
        for i in 2...(Int(sqrt(Double(n)))+1) {
            if n % i == 0 {
                return false
            }
        }
        return true
    }
 }
```

### 解法二：

厄拉多塞筛法，过滤掉所有非质数，剩下的就是质数。从2开始，过滤掉2的所有倍数，然后剩下的下一数字为3也是一个质数，继续上面的操作，直到√n。下面动图说明：[（出处）]([https://blog.csdn.net/Evan123mg/article/details/45313815](https://blog.csdn.net/Evan123mg/article/details/45313815)

![Sieve_of_Eratosthenes_animation](Images/Sieve_of_Eratosthenes_animation.gif)

```swift
 //厄拉多塞筛法
 //计算n中有多少个质数
 //从第一个质数2开始，把2的倍数全部划掉
 //下一个没被划掉的是质数3，再把3的倍数全部划掉
 //继续下一个是5，再下一个是7...直到i*i>n
 class Solution {
    func countPrimes(_ n: Int) -> Int {
        guard n > 2 else {
            return 0
        }

        var count = 2
        var nums: [Bool] = Array(repeating: true, count: n)
        nums[0] = false
        nums[1] = false
        for i in 2..<nums.count {
            if i*i > n {
                break
            }
            if nums[i] {
                var j = i*i  //为什么从i*i开始，而不是从i*2开始？因为i*i之前的数都已经计算过了，没必要从新计算，比如：i=3，3*2=6因为当i=2的时候已经计算过6了
                while j < nums.count {
                    if nums[j] {
                        count = count+1
                    }
                    nums[j] = false
                    j = j+i
                }
            }
        }
        return n-count
    }
 }
```

### 解法三：

厄拉多塞筛法实现二。

```swift
 class Solution {
    func countPrimes(_ n: Int) -> Int {
        guard n > 2 else {
            return 0
        }
        guard n > 3 else {
            return 1
        }

        var nums: [Bool] = Array(repeating: true, count: n)
        var res = n - 2
        for i in 2..<Int(sqrt(Double(n)))+1 {
            if nums[i] {
                var j = i*i  //为什么从i*i开始，而不是从i*2开始？因为i*i之前的数都已经计算过了，没必要从新计算，比如：i=3，3*2=6因为当i=2的时候已经计算过6了
                while j < n {
                    if nums[j] {
                        nums[j] = false
                        res -= 1
                    }
                    j += i
                }
            }
        }
        return res
    }
 }
```

### 解法四：

厄拉多塞筛法实现进阶，从3开始，每次都递增2，也就是跳过从4开始的所有偶数，效率提高了一半。

```swift
 class Solution {
    func countPrimes(_ n: Int) -> Int {
        guard n > 2 else {
            return 0
        }
        var nums: [Bool] = Array(repeating: true, count: n)
        var count = n / 2
        
        var i = 3 //从3开始，5，7，9，因为从3开始后面的偶数都不是质数
        while i * i < n {
            if nums[i] {
                var j = i * i
                
                while j < n {
                    if nums[j] {
                        nums[j] = false
                        count -= 1
                    }
                    j += 2 * i //跳过2的倍数，因为i跳过了2的倍数，所以，所有2的倍数的值都是true，在一开始count=n/2就已经计算了3之后所有2的倍数的，所以，这里需要跳过，不然会重复减
                }
            }
            i += 2 //跳过2的倍数
        }
        return count
    }
 }
```
