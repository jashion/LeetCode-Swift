### 题目：

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![maxArea](Images/maxArea.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 示例:

 输入: [1,8,6,2,5,4,8,3,7]
 输出: 49

### 解法一：

暴力法，找出所有的组合，然后再计算每一个组合的值，取最大值。

```swift
 class Solution {
    func maxArea(_ height: [Int]) -> Int {
        if height.count <= 1 {
            return 0
        }
        var maxArea = 0
        for i in 1..<height.count {
            for j in 0..<i {
                maxArea = max(maxArea, min(height[j], height[i])*(i-j))
            }
        }
        return maxArea
    }
 }
```

### 解法二：

神一般，时间复杂度为O(n)的双指针法。根据给出的题目，可以知道，决定与两个数值最小的那个，以及两个数值之间的距离，数值最小的越大，两个数值之间的距离越大，则整个区域面积就越大。距离，最大的是起点到终点，那么，我们需要在距离不断减少的情况下，找到最大的数值。可以生成两个指针，一个指向起点，一个指向终点。那么，问题来了，怎么保证数值的最大。是这样的，数值取决于小的那一边，也就是说，在距离逐渐减少的情况下，只有移动小，才能在数值变大的时候增大，如果移动大的那一边，无论数值变大还是变小，区域面积都会变小，因为我们需要的做的是，在距离逐步变小，取到最大的区域面积。

```swift
 class Solution {
    func maxArea(_ height: [Int]) -> Int {
        var maxArea = 0
        var i = 0
        var j = height.count-1
        while i < j {
            maxArea = max(maxArea, min(height[i], height[j])*(j-i))
            if height[i] < height[j] {
                i += 1
            } else {
                j -= 1
            }
        }
        return maxArea
    }
 }
```
