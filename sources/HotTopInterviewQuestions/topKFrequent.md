### 题目：

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

```
示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]
说明：

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
```

### 解法一：

先使用字典，以数字元素为key，数据元素出现的次数为value，统计每个数据元素的出现次数。然后，以数据元素出现的次数降序，输出前k个数据元素。

```swift
 class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        var dic = [Int:Int]()
        for num in nums {
            if dic[num] != nil {
                dic[num] = dic[num]! + 1
            } else {
                dic[num] = 1
            }
        }
        let tmp = dic.sorted { (x, y) -> Bool in
            x.value > y.value
        }
        var res = [Int]()
        for i in 0..<k {
            res.append(tmp[i].key)
        }
        return res
    }
 }
```

### 解法二：

堆排序，可以采用小顶堆。选择前k个数据元素，构建小顶堆。然后从下标k+1（小表从1开始，不是从0，比较好计算）开始，如果当前数据元素比堆顶部元素要大，则直接替换，然后再重构小顶堆，直到遍历完整个数组。然后，取前k个数据输出。

```swift
 class Solution {
    func headAdjust(sortedList: inout [[Int:Int]], index: Int, length: Int) {
        let temp = sortedList[index]
        var s = index;  //根, index从1开始算
        var j = index*2  //左子树
        while j <= length {
            if j < length && sortedList[j].values.first! > sortedList[j+1].values.first! {
                j += 1
            }
            if temp.values.first! <= sortedList[j].values.first! {
                break
            }
            sortedList[s] = sortedList[j]
            s = j;
            j = s*2  //左子树
        }
        sortedList[s] = temp
    }

    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        var dic = [Int:Int]()
        for num in nums {
            if dic[num] != nil {
                dic[num] = dic[num]! + 1
            } else {
                dic[num] = 1
            }
        }
        var tmp = [[Int:Int]]()
        for item in dic {
            if tmp.isEmpty {
                tmp.append([:])  //下标从1开始
            }
            tmp.append(Dictionary(dictionaryLiteral: item))
        }
        for i in stride(from: k/2, through: 1, by: -1) {
            headAdjust(sortedList: &tmp, index: i, length: k)
        }
        for i in k+1..<tmp.count {
            if tmp[i].values.first! > tmp[1].values.first! {
                tmp.swapAt(1, i)
                headAdjust(sortedList: &tmp, index: 1, length: k)
                print(tmp)
            }
        }
        var res = [Int]()
        for i in 1...k {  //下标从1开始
            res.append(tmp[i].keys.first!)
        }
        return res.reversed()
    }
 }
```

### 解法三：

桶排序。先用字典统计每个数据元素出现的次数，然后，以次数为下标，存对应的数据元素，最后过滤掉出现次数为0的下标，取最后面的k个数据元素。

```swift
 class Solution {
    func topKFrequent(_ nums: [Int], _ k: Int) -> [Int] {
        var dic = [Int:Int]()
        for num in nums {
            if dic[num] != nil {
                dic[num] = dic[num]! + 1
            } else {
                dic[num] = 1
            }
        }
        var list = Array<[Int]>.init(repeating: [], count: nums.count+1)
        for item in dic {
            var tmp = list[item.value]
            tmp.append(item.key)
            list[item.value] = tmp
        }
        var res = [Int]()
        var i = list.count-1
        while i >= 0, res.count < k {
            let tmp = list[i]
            if tmp.count > 0 {
                res.append(contentsOf: tmp)
            }
            i -= 1
        }
        return res
    }
 }
```
