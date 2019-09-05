### 题目：

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

示例:

```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1); // 返回 1
cache.put(3, 3); // 该操作会使得密钥 2 作废
cache.get(2); // 返回 -1 (未找到)
cache.put(4, 4); // 该操作会使得密钥 1 作废
cache.get(1); // 返回 -1 (未找到)
cache.get(3); // 返回 3
cache.get(4); // 返回 4
```

### 解法一：

暴力数组法。

```swift
 class LRUCache {
    var res = [(Int, Int)]()
    var maxCount = 0

    init(_ capacity: Int) {
        maxCount = capacity
    }

    func get(_ key: Int) -> Int {
        if let i = res.firstIndex(where: { (x) -> Bool in
            x.0 == key
        }) {
            let item = res.remove(at: i)
            res.append(item)
            return item.1
        }
        return -1
    }

    func put(_ key: Int, _ value: Int) {
        if let i = res.firstIndex(where: { (x) -> Bool in
            x.0 == key
        }) {
            res.remove(at: i)
        }
        res.append((key, value))
        if res.count > maxCount {
            res.removeFirst()
        }
    }
 }
```

### 解法二：

哈希表+双向链表法，这个方法妙在，查找和移动，删除时间复杂度都是O(1)，充分利用了哈希表查找以及双向链表插入和删除效率高的特点。

```swift
 class LRUCache {
    class ListNode {
        var key: Int
        var value: Int
        var next: ListNode?
        var pre: ListNode?
        
        init(_ key: Int, _ value: Int) {
            self.key = key
            self.value = value
        }
    }
    
    var nodes = [Int: ListNode]()
    var head: ListNode?
    var tail: ListNode?
    var capacity = 0
    
    init(_ capacity: Int) {
        self.capacity = capacity
        head = ListNode.init(0, 0)
        tail = ListNode.init(0, 0)
        head?.next = tail
        tail?.pre = head
    }
    
    func get(_ key: Int) -> Int {
        if let node = nodes[key] {
            put(key, node.value)
            return node.value
        }
        return -1
    }
    
    func put(_ key: Int, _ value: Int) {
        if let node = nodes[key] {
            //set value
            node.value = value
            //remove
            node.pre?.next = node.next
            node.next?.pre = node.pre
            //insert
            node.pre = tail?.pre
            node.next = tail
            //set tail
            tail?.pre?.next = node
            tail?.pre = node
        } else {
            print("\(nodes.count)  \(capacity)")
            if nodes.count == capacity {
                let first = head?.next
                //remove
                first?.pre?.next = first?.next
                first?.next?.pre = first?.pre
                
                nodes.removeValue(forKey: first!.key)
            }
            
            let node = ListNode.init(key, value)

            //insert
            node.pre = tail?.pre
            node.next = tail
            //set tail
            tail?.pre?.next = node
            tail?.pre = node
            
            //
            nodes[key] = node
        }
    }
 }
```


