### 题目：

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

```
示例 1:

输入: 4
输出: 2
示例 2:

输入: 8
输出: 2
说明: 8 的平方根是 2.82842...,
 由于返回类型是整数，小数部分将被舍去。
```

### 解法一：

二分法，一个整数，除了0和1，任何数的开放都小于或者等于其1/2。居于这个基础之上，采用二分法逐渐逼近真正的解。

```swift
 class Solution {
    func mySqrt(_ x: Int) -> Int {
        guard x > 0 else {
            return 0
        }
        var min: Double = 0
        var max: Double = Double(x/2 + 1)
        let xValue = Double(x)
        var mid: Double = 0
        while fabs(max - min) > 0.1 {
            mid = (max - min)/2 + min
            let result = mid*mid
            if result == xValue {
                return Int(mid)
            } else if result < xValue {
                min = mid
            } else {
                max = mid
            }
        }
        return Int(max)*Int(max) <= x ? Int(max) : Int(min)
    }
 }
```

### 解法二：

原函数法。

```swift
 class Solution {
    func mySqrt(_ x: Int) -> Int {
        guard x > 0 else {
            return 0
        }
        return Int(sqrt(Double(x)))
    }
 }
```

### 解法三：

[牛顿迭代法]([https://blog.csdn.net/ccnt_2012/article/details/81837154](https://blog.csdn.net/ccnt_2012/article/details/81837154)，通俗来讲就是使用直线代替曲线，逐渐逼近真正解的过程。求一个数的开放，其实就是求二次函数：f(x) = x^2-a的解，也就是当f(x)=0时，x的值。该函数的导数f'(x)=2x，那么任意一点a1以及经过这点的曲线的切线和x轴的交点a2有下面的关系：f(a1)/(a1-a2)=2a1，变形可得：f(a1)=(a1-a2)*2a1，即a2=a1-f(a1)/2a1，即a2=x-f(x)/2x，将f(x)=x^2-a带入可得，a2=x-(x^2-a)/2x变形可得a2=(x+a/x)/2。递归法实现牛顿迭代法：

```swift
 class Solution {
    func mySqrt(_ x: Int) -> Int {
        guard x > 0 else {
            return 0
        }
        return Int(sqrts(Double(x), Double(x)))
    }

    func sqrts(_ x: Double, _ s: Double) -> Double {
        let res = (x + s / x) / 2
        if res == x {
            return x
        } else {
            return sqrts(res, s)
        }
    }
 }
```

### 解法四：

迭代法实现牛顿迭代法。

```swift
 class Solution {
    func mySqrt(_ x: Int) -> Int {
        guard x > 0 else {
            return 0
        }
        var s = Double(x)
        while fabs(s*s-Double(x)) > 0.5 {
            s = (s+Double(x)/s)/2
        }
        return Int(s)
    }
 }
```
