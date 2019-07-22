# LeetCode-Swift

LeetCode上的算法，使用Swift实现。会根据不同的类别分类，附上自己的代码实现以及觉得比较高效，简洁和意想不到的代码实现。

<!--more-->

## 1.数据结构

#### 数组

#### 栈

#### 队列

#### 树

##### 二叉树

---

| 序号   | 标题                                                                                            | 解题思路                                                                                                                            | 难度     |
|:----:|:---------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------:|:------:|
| #671 | [合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/submissions/)                 | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MergeBinaryTree.md)                 | easy   |
| #108 | [将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/) | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/SortedArrayTransferToBinaryTree.md) | easy   |
| #101 | [对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)                                     | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/IsSymmetric.md)                     | easy   |
| #94  | [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)                   | [解法](sources/HotTopInterviewQuestions/inorderTraversal.md)                                                                      | medium |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |

#### 图

## 2.算法

#### 排序算法

---

| 序号   | 标题                                                                                   | 解题思路                                                                                                 | 难度   |
|:---- |:------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------:|:----:|
| #905 | [按奇偶排序数组](https://leetcode-cn.com/submissions/detail/19467555/)                      | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/SortAlgorithm/EvenOddArray.md)    | easy |
| #922 | [按奇偶排序数组II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)               | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/SortAlgorithm/oddAndEven.md)      | easy |
| #26  | [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/) | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/SortAlgorithm/removeRepeatNum.md) | easy |
| #977 | [有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)               | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/SortAlgorithm/SortedArrayPow.md)  | easy |
|      |                                                                                      |                                                                                                      |      |

#### 回溯算法

解题步骤：

1.针对所给问题，定义问题的解空间，至少包括问题的**一个最优解**。

2.确定**易于搜索的解空间结构**，可以使用回溯法方便的搜索整个解空间结构

3.使用深度优先(DFS)搜索解空间，并在搜索的过程中使用**剪枝函数**避免无效的搜索回

[溯法例题讲解](sources/BackTraceAlgorithm/backTrace.md)

---

| 序号  | 标题                                                             | 解题思路                                                          | 难度     |
|:--- |:--------------------------------------------------------------:|:-------------------------------------------------------------:|:------:|
| #78 | [子集](https://leetcode-cn.com/problems/subsets/)                | [解法](sources/HotTopInterviewQuestions/subsets.md)             | medium |
| #90 | [子集II](https://leetcode-cn.com/problems/subsets-ii/)           | [解法](sources/BackTraceAlgorithm/subSetsII.md)                 | medium |
| #22 | [括号生成](https://leetcode-cn.com/problems/generate-parentheses/) | [解法](sources/HotTopInterviewQuestions/generateParenthesis.md) | medium |
| #46 | [全排列](https://leetcode-cn.com/problems/permutations/)          | [解法](sources/HotTopInterviewQuestions/permute.md)             | medium |
|     |                                                                |                                                               |        |
|     |                                                                |                                                               |        |
|     |                                                                |                                                               |        |
|     |                                                                |                                                               |        |

#### 动态规划

#### 分治法

#### 贪心算法

## 3.其他专题

#### 精选Top面试题

---

| 序号   | 标题                                                                                            | 解题思路                                                                                                                            | 难度     |
|:---- |:---------------------------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------------------:|:------:|
| #237 | [删除链表中的结点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)                    | [解法（C语言）](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/DeleteLinkedNode.md)           | easy   |
| #671 | [合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/submissions/)                 | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MergeBinaryTree.md)                 | easy   |
| #344 | [反转字符串](https://leetcode-cn.com/problems/reverse-string/)                                     | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/ReverseString.md)                   | easy   |
| #171 | [Excel表序列号](https://leetcode-cn.com/problems/excel-sheet-column-number/)                      | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/ExcelSerialNumber.md)               | easy   |
| #108 | [将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/) | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/SortedArrayTransferToBinaryTree.md) | easy   |
| #118 | [杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)                                    | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/Pascal'sTriangle.md)                | easy   |
| #206 | [反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)                                 | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/ReverseLink.md)                     | easy   |
| #412 | [Fizz Buzz](https://leetcode-cn.com/problems/fizz-buzz/)                                      | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/FizzBuzz.md)                        | easy   |
| #169 | [求众数](https://leetcode-cn.com/problems/majority-element/)                                     | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MajorityNumber.md)                  | easy   |
| #13  | [罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)                                 | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/RomanToInt.md)                      | easy   |
| #21  | [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)                          | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MergeTwoSortedLinks.md)             | easy   |
| #283 | [移动零](https://leetcode-cn.com/problems/move-zeroes/)                                          | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MoveZeroes.md)                      | easy   |
| #191 | [位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)                                   | [解法（C语言）](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/HammingWeight.md)              | easy   |
| #202 | [快乐数](https://leetcode-cn.com/problems/happy-number/)                                         | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/HappyNumber.md)                     | easy   |
| #122 | [买卖股票的最佳时机II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)           | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MaxProfitII.md)                     | easy   |
| #242 | [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)                                   | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/Anagram.md)                         | easy   |
| #371 | [两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)                                | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/GetSum.md)                          | easy   |
| #268 | [缺失数字](https://leetcode-cn.com/problems/missing-number/)                                      | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MissingNumber.md)                   | easy   |
| #38  | [报数](https://leetcode-cn.com/problems/count-and-say/)                                         | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/CountAndSay.md)                     | easy   |
| #121 | [买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)                | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MaxProfit.md)                       | easy   |
| #155 | [最小栈](https://leetcode-cn.com/problems/min-stack/)                                            | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/MinStack.md)                        | easy   |
| #217 | [存在重复的元素](https://leetcode-cn.com/problems/contains-duplicate/)                               | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/ContainsDuplicate.md)               | easy   |
| #101 | [对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)                                     | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/IsSymmetric.md)                     | easy   |
| #1   | [两数之和](https://leetcode-cn.com/problems/two-sum/)                                             | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/TwoSum.md)                          | easy   |
| #70  | [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)                                      | [解法](https://github.com/jashion/LeetCode-Swift/blob/master/sources/HotTopInterviewQuestions/ClimbStairs.md)                     | easy   |
| #53  | [最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)                                   | [解法](/sources/HotTopInterviewQuestions/MaxSubArray.md)                                                                          | easy   |
| #26  | [删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)          | [解法](/sources/HotTopInterviewQuestions/RemoveDuplicates.md)                                                                     | easy   |
| #88  | [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)                              | [解法](/sources/HotTopInterviewQuestions/Merge.md)                                                                                | easy   |
| #326 | [3的幂](https://leetcode-cn.com/problems/power-of-three/)                                       | [解法](/sources/HotTopInterviewQuestions/IsPowerOfThree.md)                                                                       | easy   |
| #160 | [相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)                    | [解法（C语言）](/sources/HotTopInterviewQuestions/GetIntersectionNode.md)                                                             | easy   |
| #190 | [颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)                                      | [解法（C语言）](/sources/HotTopInterviewQuestions/ReverseBits.md)                                                                     | easy   |
| #350 | [两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)                    | [解法](/sources/HotTopInterviewQuestions/Interset.md)                                                                             | easy   |
| #198 | [打家劫舍](https://leetcode-cn.com/problems/house-robber/)                                        | [解法](/sources/HotTopInterviewQuestions/Rob.md)                                                                                  | easy   |
| #141 | [环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)                                   | [解法（C语言）](/sources/HotTopInterviewQuestions/HasCycle.md)                                                                        | easy   |
| #125 | [验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)                                   | [解法](sources/HotTopInterviewQuestions/IsPalindrome.md)                                                                          | easy   |
| #66  | [加一](https://leetcode-cn.com/problems/plus-one/)                                              | [解法](sources/HotTopInterviewQuestions/PlusOne.md)                                                                               | easy   |
| #387 | [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)          | [解法](sources/HotTopInterviewQuestions/FirstUniqChar.md)                                                                         | easy   |
| #172 | [阶乘最后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)                         | [解法](sources/HotTopInterviewQuestions/TrailingZeroes.md)                                                                        | easy   |
| #20  | [有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)                                  | [解法](sources/HotTopInterviewQuestions/IsValid.md)                                                                               | easy   |
| #28  | [实现strStr()](https://leetcode-cn.com/problems/implement-strstr/)                              | [解法](sources/HotTopInterviewQuestions/strStr.md)                                                                                | easy   |
| #189 | [旋转数组](https://leetcode-cn.com/problems/rotate-array/)                                        | [解法](sources/HotTopInterviewQuestions/rotate.md)                                                                                | easy   |
| #234 | [回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)                              | [解法](sources/HotTopInterviewQuestions/isPalindromeList.md)                                                                      | easy   |
| #69  | [x的平方根](https://leetcode-cn.com/problems/sqrtx/solution/niu-dun-die-dai-fa-by-loafer/)        | [解法](sources/HotTopInterviewQuestions/mySqrt.md)                                                                                | easy   |
| #14  | [最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)                             | [解法](sources/HotTopInterviewQuestions/longestCommonPrefix.md)                                                                   | easy   |
| #7   | [整数反转](https://leetcode-cn.com/problems/reverse-integer/)                                     | [解法](sources/HotTopInterviewQuestions/reverseNumber.md)                                                                         | easy   |
| #204 | [计数质数](https://leetcode-cn.com/problems/count-primes/)                                        | [解法](sources/HotTopInterviewQuestions/countPrimes.md)                                                                           | easy   |
| #78  | [子集](https://leetcode-cn.com/problems/subsets/)                                               | [解法](sources/HotTopInterviewQuestions/subsets.md)                                                                               | medium |
| #22  | [括号生成](https://leetcode-cn.com/problems/generate-parentheses/)                                | [解法](sources/HotTopInterviewQuestions/generateParenthesis.md)                                                                   | medium |
| #46  | [全排列](https://leetcode-cn.com/problems/permutations/)                                         | [解法](sources/HotTopInterviewQuestions/permute.md)                                                                               | medium |
| #94  | [二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)                   | [解法](sources/HotTopInterviewQuestions/inorderTraversal.md)                                                                      | medium |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |
|      |                                                                                               |                                                                                                                                 |        |

### 
