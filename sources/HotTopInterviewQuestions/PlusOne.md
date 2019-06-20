### 题目：

给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1:

```
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
```

示例 2:

```
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。
```

### 解法一：

先将最后一位+1，然后再处理进位。

```swift
  class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        guard digits.count > 0 else {
            return []
        }
        var res = digits
        var carry = 0
        res[res.count-1] = res.last!+1
        for i in (0..<res.count).reversed() {
            let tmp = res[i]+carry
            res[i] = tmp%10
            carry = tmp/10
            if carry == 0 {
                return res
            }
        }

        return res.insert(carry, at: 0)
    }
 }
```

### 解法二：

把+1放在循环内部。

```swift
 class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        guard digits.count > 0 else {
            return []
        }
        var res = digits
        for i in (0..<res.count).reversed() {
            res[i] += 1
            res[i] = res[i]%10
            if res[i] != 0 {
                return res
            }
        }
        res.insert(1, at: 0)
        return res
    }
 }
```

### 解法三：

这个是在LeetCode上看到的不一样的解法，计算进了多少位，然后就在结果数组后面补零。

```swift
 class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        if digits.count == 0 {
            return [];
        }
        var tempArr = digits;
        var resultArr : Array<Int> = [];
        var zeroCount = 0
        while tempArr.count > 0 {
            let lastInt = tempArr.last;
            if lastInt == 9 {
                zeroCount += 1;
                tempArr.removeLast();
                if tempArr.count == 0 {
                    tempArr = [1];
                    resultArr = tempArr;
                    break;
                }
            }else {
                resultArr = tempArr;
                resultArr.removeLast();
                resultArr.append(lastInt! + 1);
                break;
            }
        }
        for _ in 0..<zeroCount {
            resultArr.append(0);
        }

        return resultArr;
    }
 }
```

### 解法四：

大体上思路差不多，算法的实现上稍有差异，这里使用9来判断是否有进位。

```swift
 class Solution {
    func plusOne(_ digits: [Int]) -> [Int] {
        var result = digits
        for i in (0..<result.count).reversed() {
            if result[i] + 1 <= 9 {
                result[i] += 1
                return result
            } else {
                result[i] = 0
            }
            if i == 0 {
                result.insert(1, at: 0)
            }
        }
        return result
    }
 }
```
