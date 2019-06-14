### 题目：

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

```
push(x) -- 将元素 x 推入栈中。
pop() -- 删除栈顶的元素。
top() -- 获取栈顶元素。
getMin() -- 检索栈中的最小元素。
```
示例:

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

### 解法一：

用一个属性记录最小值。

```swift
class MinStack {
    var data: [Int] = []
    var minValue = Int.min
    /** initialize your data structure here. */
    init() {

    }

    func push(_ x: Int) {
        data.append(x)
        if x > minValue {
            minValue = x
        }
    }

    func pop() {
        guard data.count > 0 else {
            return
        }
        let value = data.removeLast()
        if minValue == value && !data.contains(minValue) {
            minValue = Int.min
        }
    }

    func top() -> Int {
        return data.last!
    }

    func getMin() -> Int {
        if minValue != Int.min {
            return minValue
        }
        minValue = data.min()!
        return minValue
    }
}
```

### 解法二：

用一个数组，记录Push过程中所有的最小值。

```swift
class MinStack {
    var data: [Int] = []
    var minList: [Int] = []
    /** initialize your data structure here. */
    init() {
        
    }
    
    func push(_ x: Int) {
        data.append(x)
        if minList.count == 0 {
            minList.append(x)
        } else {
            let min = getMin()
            if x <= min {
                minList.append(x)
            }
        }
    }
    
    func pop() {
        guard data.count > 0 else {
            return
        }
        let num = data.removeLast()
        let min = getMin()
        if num == min {
            minList.removeLast()
        }
    }
    
    func top() -> Int {
        return data.last ?? 0
    }
    
    func getMin() -> Int {
        return minList.last ?? 0
    }
}
```
