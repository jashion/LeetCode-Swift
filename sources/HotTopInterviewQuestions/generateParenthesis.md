### 题目：

给出 n 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且有效的括号组合。

```
例如，给出 n = 3，生成结果为：

[
"((()))",
"(()())",
"(())()",
"()(())",
"()()()"
]
```

### 解法一：穷举法

将n对括号所有的序列列举出来，然后过滤掉无效的序列。

```
比如: n = 2
            ""
           /   \
         "("   ")"
        /   \
      "("   ")"
     /   \
   "("   ")"
  /   \
"("   ")"  ...
```

遍历整棵二叉树，获取所有从根到叶结点的路径，然后从这些路径筛选出有效的序列。

```swift
 class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var res = [String]()
        inOrder(n*2, 0, "", &res)
        return res
    }

    func inOrder(_ n: Int, _ i: Int, _ sub: String, _ res: inout [String]) {
        if i >= n {
            if islegal(sub) {
                res.append(sub)
            }
            return
        }
        var tmp = sub
        tmp.append("(")
        inOrder(n, i+1, tmp, &res)
        tmp.removeLast()
        tmp.append(")")
        inOrder(n, i+1, tmp, &res)
    }

    func islegal(_ str: String) -> Bool {
        var stack = [Character]()
        for ch in str {
            if stack.isEmpty {
                if ch == ")" {
                    return false
                } else {
                    stack.append(ch)
                }
            } else if ch == "(" {
                stack.append(ch)
            } else if ch == ")" {
                let last = stack.removeLast()
                if last != "(" {
                    return false
                }
            }
        }
        return stack.isEmpty
    }
 }
```

### 解法二：穷举法优化

思路和解法一一样，都是穷举法，但是，不是遍历到了叶子结点才判断该路径是否符合条件，每次在添加结点的时候都会判断，可以跳过多余的结点。

实现1:

```swift
 class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        guard n > 0 else {
            return []
        }
        if n == 1 {
            return ["()"]
        }
        var res = [String]()
        inOrder(n*2, 1, &res, "", "(")
        inOrder(n*2, 1, &res, "", ")")
        return res
    }
    func inOrder(_ count: Int, _ i: Int, _ res: inout [String], _ sub: String, _ appenStr: String) {
        var tmp = sub
        tmp.append(appenStr)
        if !isEligible(tmp, count) {
            return
        }
        if i >= count {
            res.append(tmp)
            return
        }
        inOrder(count, i+1, &res, tmp, "(")
        inOrder(count, i+1, &res, tmp, ")")
    }
    func isEligible(_ str: String, _ count: Int) -> Bool {
        if str.hasPrefix(")") {
            return false
        } else if str.count == count {
            if str.hasSuffix("(") {
                return false
            } else {
                var tmp: [Character] = []
                for item in str {
                    if tmp.isEmpty {
                        tmp.append(item)
                    } else if item == "(" {
                        tmp.append(item)
                    } else if item == ")" {
                        if tmp.last! == "(" {
                            tmp.removeLast()
                        } else {
                            return false
                        }
                    }
                }
                return tmp.isEmpty ? true : false
            }
        }
        return true
    }
 }
```

实现2:

```swift
 class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var res = [String]()
        inOrder(n*2, 0, "", &res)
        return res
    }

    func inOrder(_ n: Int, _ i: Int, _ sub: String, _ res: inout [String]) {
        if !islegal(n, sub) {
            return
        }
        if i >= n {
            if islegal(n, sub) {
                res.append(sub)
            }
            return
        }
        var tmp = sub
        tmp.append("(")
        inOrder(n, i+1, tmp, &res)
        tmp.removeLast()
        tmp.append(")")
        inOrder(n, i+1, tmp, &res)
    }

    func islegal(_ n: Int, _ str: String) -> Bool {
        var stack = [Character]()
        for ch in str {
            if stack.isEmpty {
                if ch == ")" {
                    return false
                } else {
                    stack.append(ch)
                }
            } else if ch == "(" {
                stack.append(ch)
            } else if ch == ")" {
                let last = stack.removeLast()
                if last != "(" {
                    return false
                }
            }
        }
        if stack.count > (n-str.count) {
            return false
        } else {
            return true
        }
    }
 }
```

### 解法三：

回溯法，只有在我们知道序列仍然保持有效时，才添加"("或者")"。

```swift
 class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var res = [String]()
        backTrace(n, 0, 0, "", &res)
        return res
    }

    func backTrace(_ max: Int, _ open: Int, _ close: Int, _ sub: String, _ res: inout [String]) {
        if sub.count == max*2 {
            res.append(sub)
            return
        }

        if open < max { //先添加"("括号，因为第一个肯定为"("
            var tmp = sub
            tmp.append("(")
            backTrace(max, open+1, close, tmp, &res)
        }
        if close < open { //符合条件在添加")"括号
            var tmp = sub
            tmp.append(")")
            backTrace(max, open, close+1, tmp, &res)
        }
    }
 }
```

### 解法四：闭合数

闭合数

有效的序列：第一个必定为"("

那么在1~n-1的地方必定有一个“)”与之配对

那么整个有效序列可以表示为："(" + left + ")" + right （left和right一定是有效序列，并且可以为空）

因为已经设定一对“()”，所以left(或者right)只能为空或者n-1的有效序列

```swift
 class Solution {
    func generateParenthesis(_ n: Int) -> [String] {
        var res = [String]()
        if n == 0 {
            res.append("")
        } else {
            for i in 0..<n {
                for left in generateParenthesis(i) {
                    for right in generateParenthesis(n-1-i) {
                        res.append("(" + left + ")" + right)
                    }
                }
            }
        }
        return res
    }
 }
```
