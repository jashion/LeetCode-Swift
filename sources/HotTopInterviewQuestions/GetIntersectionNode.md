### 题目：

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表：

![link1](Images/link1.png)

在节点 c1 开始相交。

示例 1：

![link2](Images/link2.png)

输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

示例 2：

![link3](Images/link3.png)

输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。

示例 3：

![link4](Images/link4.png)

输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。

注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

（因为这道题LeetCode上面没有Swift的版本，所以下面统一使用C语言实现）

### 解法一：使用数组或者Set(或者字典)

数组：分别遍历两个链表，将链表的每个结点按照遍历的顺序存在数组里，然后从两个数组倒序查找相等的结点，如果最后一个结点不想等，则两个链表没有相交；否则，继续往前找，最前面一个相等的结点就是第一个相交结点。

Set：将第一个链表的所有结点存在Set里面，然后再遍历第二个链表，查找当前结点是否存在Set里面，如果存在，则相交，并且该结点就是第一个相交结点，直接返回该结点；如果所有都不存在，则不相交。

###### 注意⚠️：因为C语言使用数组和Set实现起来比较麻烦，并且不符合空间复杂度O(1)的要求，所以，这里只提供思路，具体实现自己去写一写。

### 解法二：遍历法

分别遍历两个链表，得到两个链表的长度L1和L2，然后得到最长的链表，比如L1。因为，如果两个链表相交（没有环），则两个链表从相交的结点开始，所有结点都一样，所以，L1链表从(L1-L2)开始遍历，同步遍历两个链表，比较是否是同一个结点，如果是，则相交，返回第一个相交结点；如果都不相等，则两个链表不相交。

```c
struct ListNode {
    int val;
    struct ListNode *next;
};

struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *aNode = headA;
    struct ListNode *bNode = headB;
    int aLength = 0;
    int bLength = 0;
    while (aNode) {
        aLength++;
        aNode = aNode->next;
    }
    aNode = headA;
    while (bNode) {
        bLength++;
        bNode = bNode->next;
    }
    bNode = headB;
    if (aLength > bLength) {
        int i = 0;
        while (i < aLength-bLength) {
            aNode = aNode->next;
            i++;
        }
    } else if (aLength < bLength) {
        int i = 0;
        while (i < bLength-aLength) {
            bNode = bNode->next;
            i++;
        }
    }
    while (aNode && bNode && aNode != bNode) {
        aNode = aNode->next;
        bNode = bNode->next;
    }
    return aNode;
}
```

### 解法三：双指针遍历法

解题思路和解法二一样，但是实现更巧妙。使用双指针，分别遍历两个链表，等比较短的链表，遍历到最后一个结点，则该指针指向比较长的链表，这时候两个指针指向同一个链表，并且相差的距离为两个链表的长度之差，然后最长的链表遍历到最后结点，将该指针指向比较短的链表，这时候，比较短的链表指针指向第一个结点，比较长的链表的指针指向两个链表的长度之差之后的第一个结点。这时候开始同步比较当前结点是否相等，如果两个链表不相交，则会返回Null，否则返回第一个相交的结点。

```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *aNode = headA;
    struct ListNode *bNode = headB;
    while (aNode != bNode) {
        if (!aNode) {
            aNode = headB;
        } else {
            aNode = aNode->next;
        }
        if (!bNode) {
            bNode = headA;
        } else {
            bNode = bNode->next;
        }
    }
    return aNode;
}
```

### 解法四：使用快慢指针

将第一个链表最后的结点的next指向第二个链表的head，如果两个链表有相交，则会形成环。问题就换成，判断链表是否有环，如果有，则返回环的入口结点。

其他知识：

1.为什么快慢指针可以检测链表是否有环？

因为slow指针每次只走一步，而fast指针每次只走两步，那么，slow指针走完整个链表，fast指针则走完两个长度的链表，两个指针肯定会相遇。可以理解完操场跑步，因为是环形，一起跑，在足够长的时间，快的人肯定会再次遇到慢的人。但是，这里有点区别，因为数字是离散的，而跑步是连续的。可以这样想，相对速度，因为fast=2slow，假设slow和fast相距n，并且fast相对于slow的速度为1，等同于slow不动，fast每次移动一步，则在n步后，肯定和slow相遇。这个推到任意速度也成立，比如slow=1，fast=3，那么相对速度为2，那么假如slow和fast想距n为奇数，不整除怎么办？没问题，一圈不行多走几圈嘛。比如2n/2=n，不就整除了。但是，slow=1和fast=2是最快能找到相遇点的速度，肯定在一个链表长度之内。

2.怎么计算入口结点？

假设slow走了s长度，则fast走了2s，并且环的长度为r，它们在环内的某点相遇，则：

2s = s + nr（n>=1，因为第一次相遇，距相遇点fast可能走了n圈）

s=nr

假设从起点都入口点的距离为a，从入口点到相遇点的距离为x，整个链表的长度为L，则：

a+x=s

a+x=nr

a=nr-x

a=(n-1)r+r-x

a=(n-1)r+L-a-x

则从起点到入口点的距离a，等于从第一次相遇点到入口点和(n-1)个环的长度。

因为从相遇点，无路走多少个环的长度都回到相遇点，所以，也就是说，从相遇点到入口点的距离等于从起点都入口点的距离，那么，我们可以使用双指针，一个指向起点，一个指向相遇点，每次每个指针都只走一步，当两个指针相遇时，则当前结点就是环的入口点。

```c
//使用快慢指针
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    if (!headA || !headB) {
        return NULL;
    }
    //遍历获取a链表的最后一个结点
    struct ListNode *aNode = headA;
    while (aNode->next) {
        aNode = aNode->next;
    }
    //将a链表的最后一个结点的下一个结点指到b链表
    aNode->next = headB;

    //使用快慢指针检验是否有环
    struct ListNode *tmpA = headA;
    struct ListNode *tmpB = headA;
    struct ListNode *meet = NULL;
    while (tmpA->next&&tmpB->next->next) {
        tmpA = tmpA->next;
        tmpB = tmpB->next->next;
        if (tmpA == tmpB) {
            meet = tmpA;
            break;
        }
    }
    //没有环，断开a链表和b链表的连接
    if (!meet) {
        aNode->next = NULL;
        return NULL;
    }
    //有环，找出环的入口点
    tmpA = headA;
    tmpB = meet;
    while (tmpA&&tmpB) {
        if (tmpA == tmpB) {
            aNode->next = NULL;
            return tmpA;
        }
        tmpA = tmpA->next;
        tmpB = tmpB->next;
    }
    return NULL;
}
```
