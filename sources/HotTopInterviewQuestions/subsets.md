### 题目：

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

```
示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

### 解法一：

穷举法，可以将所有可能构造一棵树：

```
          root
        /  \  \
        1   2  3
       / \   \
      2   3   3
     /
    3    
```

然后按层遍历:

```
第0层：[]
第1层：[1], [2], [3]
第2层：[1, 2], [1, 3], [2, 3]
第3层：[1, 2, 3]
```

那么，所有可能的解为：[[], [1], [2], [3], [1, 2], [1, 3], [2, 3], [1, 2, 3]]

```swift
 class Solution {
    class Node {
        var val: Int?
        var next: [Node] = []
        init(_ val: Int?) {
            self.val = val
        }
    }

    func setTree(_ node: inout Node, _ nums: [Int]) {
        var i = 0
        if node.val != nil {
            i = nums.firstIndex(of: node.val!)!+1
        }
        print("i: \(i)")
        while i < nums.count {
            var item = node.next[nums.count-1-i]
            var j = nums.firstIndex(of: item.val!)!+1
            while j < nums.count {
                item.next.append(Node.init(nums[j]))
                j += 1
            }
            setTree(&item, nums)
            i += 1
        }
    }

    func getResult(_ deep: Int, _ root: Node) -> [[Int]] {
        var res: [[Int]] = [[]]
        guard deep > 0 else {
            return res
        }

        for i in 1...deep {
            var tmp = Array<[Node]>()
            for _ in 1...i {
                if tmp.isEmpty {
                    tmp.append(contentsOf: root.next.map{[$0]})
                } else {
                    var tmp2 = Array<[Node]>()
                    for item in tmp {
                        if item.last?.next.count == 0 {
                            continue
                        }
                        for item2 in item.last!.next {
                            var value = item
                            value.append(item2)
                            tmp2.append(value)
                        }
                    }
                    tmp = tmp2
                }
            }
            res.append(contentsOf: tmp.map{$0.map{$0.val!}})
        }

        return res
    }

    func subsets(_ nums: [Int]) -> [[Int]] {
        guard nums.count > 0 else {
            return [[]]
        }

        var root = Node.init(nil)
        root.next.append(contentsOf: nums.map{Node.init($0)})
        setTree(&root, nums)

        return getResult(nums.count, root)
    }
 }
```

### 解法二：

回溯法，用的树结构也是解法一里面的树机构，只不过是一棵虚拟的树，不需要真正构造出来，使用递归解答。

```
bt: i->0 sub->[] res->[[]]
bt: i->1 sub->[1] res->[[], [1]]
bt: i->2 sub->[1, 2] res->[[], [1], [1, 2]]
bt: i->3 sub->[1, 2, 3] res->[[], [1], [1, 2], [1, 2, 3]]
bt: i->3 sub->[1, 3] res->[[], [1], [1, 2], [1, 2, 3], [1, 3]]
bt: i->2 sub->[2] res->[[], [1], [1, 2], [1, 2, 3], [1, 3], [2]]
bt: i->3 sub->[2, 3] res->[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3]]
bt: i->3 sub->[3] res->[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

递归实现比较难理解，可以简单的从解法一的树机构来理解，当i=0时，可能的值为1,2,3，当i=1是，可能的值为2,3，当i=2时，可能的值为3，当i=3时，直接返回，具体需要将每一步的结果和参数打印出来，理清递归之间的调用关系。

```swift
//回溯
 class Solution {
    func subsets(_ nums: [Int]) -> [[Int]] {
        /*
         回溯
         集合中每个元素的选和不选，构成了一个满二叉状态树，比如，左子树是不选，右子树是选，从根节点、到叶子节点的所有路径，构成了所有子集。通过回溯，跳过一些节点
         */
        var res : [[Int]] = [[Int]]()
        var sub : [Int] = [Int]()
        backtrace(nums, 0, &sub, &res)
        return res
    }
    private func backtrace(_ nums : [Int], _ i : Int, _ sub : inout [Int], _ res : inout [[Int]]){
        res.append(sub)
        var j = i
        while j < nums.count {
            sub.append(nums[j])
            backtrace(nums, j+1, &sub, &res)
            sub.remove(at: sub.count-1)
            j += 1
        }
    }
 }
```

### 解法三：

和解法二一样的回溯法，但是构造的结果树不一样：

```
       root
     /     \
    0       1
  /  \     /  \
  0   1    0   1 
/  \ / \  / \  / \
0  1 0  1 0  1 0 1
从根结点遍历到叶结点：
[0, 0, 0]
[0, 0, 1]
[0, 1, 0]
[0, 1, 1]
[1, 0, 0]
[1, 0, 1]
[1, 1, 0]
[1, 1, 1]
```

一共三层，每一个结点对应数组里面的一个数，0表示不加入，1表示加入，那么总共有8中可能，分别是：[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [3], [1, 2, 3]

```swift
 //可以看作，当nums为空时，res返回的是什么，然后在这基础之上，逐个添加数据元素
 //空集的幂集只有空集，每增加一个元素，让之前幂集中的每个集合，追加这个元素，就是新增的子集
 //初始化res=[[]]
 //当nums=[1]  => 往前面结果添加1 => [] [1]
 //当nums=[1, 2] => 往前面的结果添加2 => [] [1] [2] [1, 2]
 //当nums=[1, 2, 3] => 往前面的结果添加3 => [] [1] [2] [1, 2] [3] [1, 3] [2, 3] [1, 2, 3]
 //动态规划
 //res = [[]] n = 0
 //res = f(0) + f(0)*nums[n] n n > 0
 class Solution {
    func subsets(_ nums: [Int]) -> [[Int]] {
        guard nums.count > 0 else {
            return [[]]
        }
        var res: [[Int]] = [[]]
        for item in nums {
            let count = res.count
            for i in 0..<count {
                var tmp = res[i]
                tmp.append(item)
                res.append(tmp)
            }
        }
        return res
    }
 }
```
