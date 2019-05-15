解题思路：

先把数据的奇数和偶数分成两个数组，然后分别不断从两个数组取一个元素加入结果数组。

```swift
class Solution {
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var odd = [Int]()
        var even = [Int]()
        for num in A {
            if num%2 == 0 {
                even.append(num)
            } else {
                odd.append(num)
            }
        }
        var result = Array.init(repeating: 0, count: A.count)
        
        for (index, value) in odd.enumerated() {
            result[1+index*2] = value
        }

        for (index, value) in even.enumerated() {
            result[index*2] = value
        }
        return result
    }
}
```

更高效的解法：

使用奇偶双下标法，一次遍历就可以得到结果数据。

```swift
class Solution {
    func sortArrayByParityII(_ A: [Int]) -> [Int] {
        var result = Array.init(repeating: 0, count: A.count)
        var odd = 0
        var even = 0
        for num in A {
            if num%2 == 0 {
                result[even*2] = num
                even = even+1
            } else {
                result[1+odd*2] = num
                odd = odd+1
            }
        }
        return result
    }
}
```
