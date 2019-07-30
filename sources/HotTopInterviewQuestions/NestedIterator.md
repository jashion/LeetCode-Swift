### 题目：

给定一个嵌套的整型列表。设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的项或者为一个整数，或者是另一个列表。

```
示例 1:

输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
示例 2:

输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,4,6]。
```

主要有两种实现思路：

第一种，一次性降维，把嵌套的多维数组，整合成一维数组。

第二种，每读取一个，就把该数组降维。

### 解法一：

在初始化的时候，把传入的数组，全部降维为一维数组，然后，再根据下标读取相关数据。

```swift
  //递归1
  class NestedIterator {
     var nestedList = [NestedInteger]()
     var index: Int = -1
 
     init(_ nestedList: [NestedInteger]) {
         traverseNestedList(nestedList)
     }
 
     func traverseNestedList(_ nestedList: [NestedInteger]){
         for item in nestedList {
             if item.isInteger() {
                 self.nestedList.append(item)
             } else {
                 traverseNestedList(item.getList())
             }
         }
     }
 
     func next() -> Int {
         if hasNext() {
             index += 1
             return nestedList[index].getInteger()
         } else {
             return -1
         }
     }
 
     func hasNext() -> Bool {
         if index >= nestedList.count - 1 {
             return false
         } else {
             return true
         }
     }
  }

 //递归2
 class NestedIterator {
    var res = [Int]()
    var cur: Int = -1

    init(_ nestedList: [NestedInteger]) {
        traverseNestedList(nestedList)
    }

    func traverseNestedList(_ nestedList: [NestedInteger]){
        for item in nestedList {
            if item.isInteger() {
                res.append(item.getInteger())
            } else {
                traverseNestedList(item.getList())
            }
        }
    }

    func next() -> Int {
        cur += 1
        return res[cur]
    }

    func hasNext() -> Bool {
        return cur < res.count-1
    }
 }
 
 //递归3
 class NestedIterator {
    var res = [Int]()
    var cur: Int = -1

    init(_ nestedList: [NestedInteger]) {
        res = traverseNestedList(nestedList)
    }

    func traverseNestedList(_ nestedList: [NestedInteger]) -> [Int] {
        var result = [Int]()
        for item in nestedList {
            if item.isInteger() {
                result.append(item.getInteger())
            } else {
                result.append(contentsOf: traverseNestedList(item.getList()))
            }
        }
        return result
    }

    func next() -> Int {
        cur += 1
        return res[cur]
    }

    func hasNext() -> Bool {
        return cur < res.count-1
    }
 }
```

### 解法二：

不是在初始化的时候降维，而是在读取的时候降维。

```swift
 //迭代
 class NestedIterator {
    var nestedList: [NestedInteger]
    var index: Int = -1

    init(_ nestedList: [NestedInteger]) {
        self.nestedList = nestedList
    }

    func next() -> Int {
        if !hasNext() {
            return -1
        }
        index += 1
        return nestedList[index].getInteger()
    }

    func hasNext() -> Bool {
        if index >= nestedList.count-1 {
            return false
        } else {
            let item = nestedList[index+1]
            if item.isInteger() {
                return true
            }
            let tmp = item.getList()
            if tmp.count == 0 {
                nestedList.remove(at: index+1)
                return hasNext()
            } else {
                nestedList.remove(at: index+1)
                nestedList.insert(contentsOf: tmp, at: index+1)
                return hasNext()
            }
        }
    }
 }
```
