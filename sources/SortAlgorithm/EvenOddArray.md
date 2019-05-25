### 题目：

给定一个非负整数数组  `A`，返回一个数组，在该数组中， `A`  的所有偶数元素之后跟着所有奇数元素。

你可以返回满足此条件的任何数组作为答案。

### 解题思路：

两个指针，i=0，j=A.count-1，然后遍历数组A，如果A[index]%2==0则表示是偶数，则result[i]=A[index]，并且i++，同理，A[index]%2==1则表示是奇数，则result[j]=A[index]，并且j--，直到遍历整个数组。

```swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        var result = Array.init(repeating: 0, count: A.count)
        var i = 0, j = result.count-1
        for k in 0..<A.count {
            let item = A[k]
            if item%2 == 0 {
                result[i] = item
                i = i+1
            } else {
                result[j] = item
                j = j-1
            }
        }
        return result
    }
}
```

### 升级版：

如果数组A已经满足要求的输出，那么上面的解法可以更高效。

```swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        var result = Array.init(A)
        var i = 0, j = result.count-1
        while i < j {
            if result[i]%2 == 0 {
                i = i+1
            } else if result[j]%2 == 1 {
                j = j-1
            } else {
                result.swapAt(i, j)
                i = i+1
                j = j-1
            }
        }
        return result
    }
}
```

### 更简洁的解法：

```swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        let even = A.filter { (value) -> Bool in
            return value%2 == 0
        }
        let odd = A.filter { (value) -> Bool in
            return value%2 == 1
        }
        return even+odd
    }
}
```

### 巧妙的解法：

```swift
class Solution {
    func sortArrayByParity(_ A: [Int]) -> [Int] {
        var result = [Int]()
        for (_, value) in A.enumerated() {
            var atIndex = 0
            if value%2 != 0 {
                atIndex = result.count
            }
            result.insert(value, at: atIndex)
        }
        return result
    }
}
```


