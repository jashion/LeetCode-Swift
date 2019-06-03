### 题目：

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。

说明：

- 链表至少包含两个节点。
- 链表中所有节点的值都是唯一的。
- 给定的节点为非末尾节点并且一定是链表中的一个有效节点。
- 不要从你的函数中返回任何结果。

### 解题思路：

从说明里面可以得到一些关键的信息，不是空链表，删除的结点有效并且不是尾结点，最后不需要返回，所以，只要找到结点删除就好了。

```c
void deleteNode(struct ListNode* node) {
    struct ListNode *nextNode = node->next;
    node->val = nextNode->val;
    node->next = nextNode->next;
    free(nextNode);
}
```
