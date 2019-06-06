### 题目：

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

### 解题思路一：

遍历数组，遇到零则先移除，再在后尾追加一个零。

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var i = 0
        var count = nums.count
        while i < count {
            if nums[i] == 0 {
                nums.remove(at: i)
                nums.append(0)
                count = count-1
            } else {
                i = i+1
            }
        }
    }
}
```

##### 算法简化：

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        for i in (0..<nums.count).reversed() {
            if nums[i] == 0 {
                nums.remove(at: i)
                nums.append(0)
            }
        }
    }
}
```

### 解题思路二：

使用双下标遍历数组，依次按顺序将每次遇到的零和非零交换。

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var zeroIndex = -1;

        var i = 0;
        while(i < nums.count) {
            if (nums[i] != 0) {
                if (zeroIndex >= 0) {
                    nums[zeroIndex] = nums[i];
                    nums[i] = 0;

                    zeroIndex = zeroIndex + 1;
                }
            } else if (zeroIndex < 0) {
                zeroIndex = i;
            }
            i = i + 1;
        }
    }
}
```

##### 算法简化：

使用for循环替代while循环

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        if nums.isEmpty {
            return
        }
        var index = -1
        for i in 0 ..< nums.count {
            if nums[i] != 0 {
                if index >= 0 {
                    nums[index] = nums[i]
                    nums[i] = 0
                    index += 1
                }
            }else if index < 0 {
                index = i
            }
        }
    }
}
```

### 解题思路三：

将所有非零的数往前移，并记录非零数值的个数j，然后，从末尾开始，将n-j个数赋值为零。

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var i = 0
        var j = 0
        while i < nums.count {
            if nums[i] != 0 {
                nums[j] = nums[i]
                j += 1
            }
            i += 1
        }

        while j < nums.count {
            nums[j] = 0
            j += 1
        }
    }
}
```

##### 另外的实现：

使用for循环替代while循环。

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        if nums.isEmpty {
            return;
        }
        var k = 0
        for i in 0 ..< nums.count {
            if (nums[i] != 0) {
                nums[k] = nums[i]
                k = k + 1
            }
        }
        for i in k ..< nums.count {
            nums[i] = 0;
        }
    }
}
```

##### 算法简化：

```swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        let count = nums.count
        nums = nums.filter { $0 != 0 }
        nums += [Int](repeating: 0, count: count - nums.count)
    }
}
```


