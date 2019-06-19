### 题目：

给定两个数组，编写一个函数来计算它们的交集。

示例 1:

```
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
```
示例 2:

```
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
```
说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。

进阶:

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

### 解法一：暴力法

遍历其中一个数组，然后寻找该数组的元素是否包含在另外一个数组，如果有，则将这个元素添加进去结果数组，然后删除另外一个数组中包含的该元素。

```swift
class Solution {
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var res: [Int] = []
        var tmp1 = nums1
        var tmp2 = nums2
        var count = tmp1.count
        var i = 0
        while i < count {
            if let j = tmp2.firstIndex(of: tmp1[i]) {
                res.append(tmp2[j])
                tmp1.remove(at: i)
                tmp2.remove(at: j)
                count -= 1
            } else {
                i += 1
            }
        }
        return res
    }
}
```

### 解法二：字典法

遍历一个数组，用数据元素作为key，数据元素在数组出现的次数作为value，存入字典中。然后，遍历另外一个数组，查看字典中是否存在该元素并且对应的value为不为0，则将该数据添加进结果数组，并且字典中对应的key的value减一。

```swift
class Solution {
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var res: [Int] = []
        var tmp1 = nums1.sorted()
        var tmp2 = nums2.sorted()
        var i = 0
        var j = 0
        while i < tmp1.count, j < tmp2.count {
            if tmp1[i] > tmp2[j] {
                j += 1
            } else if tmp1[i] < tmp2[j] {
                i += 1
            } else {
                res.append(tmp1[i])
                i += 1
                j += 1
            }
        }
        return res
    }
}
```

### 解法三：先排序，后遍历

两个数组先从小到大排序，然后双指针遍历比较。

```swift
class Solution {
    func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
        var res: [Int] = []
        if nums1.isEmpty || nums2.isEmpty {
            return res
        }
        if nums1.min()! > nums2.max()! || nums2.min()! > nums1.max()! {
            return res
        }
        var tmp1 = nums1.sorted()
        var tmp2 = nums2.sorted()
        var i = 0
        var j = 0
        while i < tmp1.count, j < tmp2.count {
            if tmp1[i] > tmp2[j] {
                j += 1
            } else if tmp1[i] < tmp2[j] {
                i += 1
            } else {
                res.append(tmp1[i])
                i += 1
                j += 1
            }
        }
        return res
    }
}
```
