### 题目：

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

![robot_maze](images/robot_maze.png)

问总共有多少条不同的路径？

```
例如，上图是一个7 x 3 的网格。有多少可能的路径？

说明：m 和 n 的值均不超过 100。

示例 1:

输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
示例 2:

输入: m = 7, n = 3
输出: 28
```

### 解法一：

动态规划递归法。

1.边界：f(1, 1) = 1 f(2, 1) = f(1, 2) = 1

2.最优子结构：f(m, n) = f(m-1, n) + f(m, n-1)

3.传递方程：

f(m, n) :  f(1, 1) = 1 (m=1, n=1)

                f(2, 1) = f(1, 2) = 1   (m=1/2, n=1/2)

                f(m, n) = f(m-1, n) + f(m, n-1)  (m > 1, n > 1)

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        if m == 0 || n == 0 {
            return 0
        }
        if m == 1, n == 1 {
            return 1
        }
        return uniquePaths(m-1, n) + uniquePaths(m, n-1)
    }
 }
```

### 解法二：

动态规划缓存递归法。把解法一中重复的计算缓存起来，节省时间，提高效率。

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var res = Array.init(repeating: Array.init(repeating: 0, count: n+1), count: m+1)
        return allPaths(m, n, &res)
    }

    func allPaths(_ m: Int, _ n: Int, _ res: inout [[Int]]) -> Int {
        if m == 0 || n == 0 {
            return 0
        }

        if m == 1, n == 1 {
            return 1
        }

        if res[m][n] > 0 {
            return res[m][n]
        }

        res[m][n-1] = allPaths(m, n-1, &res)
        res[m-1][n] = allPaths(m-1, n, &res)
        res[m][n] = res[m][n-1] + res[m-1][n]
        return res[m][n]
    }
 }
```

### 解法三：

动态规划，迭代法。使用二维数组缓存计算结果。

以m=4, n= 4为例：

| 1   | 1   | 1   | 1   |
| --- | --- | --- | --- |
| 1   | 2   | 3   | 4   |
| 1   | 3   | 6   | 10  |
| 1   | 4   | 10  | 20  |

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var res = Array.init(repeating: Array.init(repeating: 1, count: n), count: m)
        var i = 1, j = 1
        while i < m {
            j = 1
            while j < n {
                res[i][j] = res[i - 1][j] + res[i][j - 1]
                j += 1
            }
            i += 1
        }
        return res[m-1][n-1]
    }
 }
```

### 解法四：

动态规划迭代法，使用双数组。

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var pre = Array.init(repeating: 1, count: n)
        var cur = Array.init(repeating: 1, count: n)
        var i = 1, j = 1
        while i < m {
            j = 1
            while j < n {
                cur[j] = cur[j-1] + pre[j]
                j += 1
            }
            pre = cur
            i += 1
        }
        return pre[n-1]
    }
 }
```

### 解法五：

动态规划迭代法，使用一维数组。

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var cur = Array.init(repeating: 1, count: n)
        var i = 1, j = 1
        while i < m {
            j = 1
            while j < n {
                cur[j] = cur[j] + cur[j-1]
                j += 1
            }
            i += 1
        }
        return cur[n-1]
    }
 }
```

### 解法六：

排列组合法。向下走几步和向右走几步，都是固定的，比如：m=3, n=2，则需要向右走2步，向下走1步。也就C(3, 2)，等同于在3个步骤里，选出两个步骤的组合，公式为：C(m+n-2, m-1) = factorial(m+n-2)/factorial(m-1)/factorial(n-1)。

```swift
 class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        return factoral(m+n-2)/factoral(m-1)/factoral(n-1)
    }

    func factoral(_ n: Int) -> Int {
        var res = 1
        var i = 1
        while i <= n {
            res *= i
            i += 1
        }
        return res
    }
 }
```


