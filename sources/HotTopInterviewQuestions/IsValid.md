### 题目：

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

```
输入: "()"
输出: true
```
示例 2:

```
输入: "()[]{}"
输出: true
```
示例 3:

```
输入: "(]"
输出: false
```
示例 4:

```
输入: "([)]"
输出: false
```
示例 5:

```
输入: "{[]}"
输出: true
```

### 解法一：

思路使用栈匹配。

```swift
 class Solution {
    func isValid(_ s: String) -> Bool {
        guard !s.isEmpty else {
            return true
        }
        guard s.count%2 == 0 else {
            return false
        }
        var stack: String = ""
        for value in s {
            if value == "(" || value == "[" || value == "{" {
                stack.append(value)
            } else if stack.last == "(", value == ")" {
                stack.removeLast()
            } else if stack.last == "[", value == "]" {
                stack.removeLast()
            } else if stack.last == "{", value == "}" {
                stack.removeLast()
            } else {
                return false
            }
        }
        return stack.isEmpty
    }
 }
```

### 解法二：

思路也是使用栈匹配，但是其中使用了字典。

```swift
 class Solution {
    func isValid(_ s: String) -> Bool {
        guard s.count%2 == 0 else {
            return false
        }
        var match: [Character: Character] = [")":"(", "]":"[", "}":"{"]
        let leftValue = match.values
        var stack = [Character]()
        for value in s {
            if leftValue.contains(value) {
                stack.append(value)
                continue
            }
            if match[value] == nil {
                return false
            }
            if stack.last != match[value] {
                return false
            }
            stack.removeLast()
        }
        return stack.isEmpty
    }
 }
```
