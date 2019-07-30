### 题目：

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

```
示例:

Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
说明:

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。
```

### 解法一：

使用Set。

```swift
class Trie {
    var res = Set<String>()
    
    /** Initialize your data structure here. */
    init() {
    }
    
    /** Inserts a word into the trie. */
    func insert(_ word: String) {
        if !res.contains(word) {
            res.insert(word)
        }
    }
    
    /** Returns if the word is in the trie. */
    func search(_ word: String) -> Bool {
        return res.contains(word)
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    func startsWith(_ prefix: String) -> Bool {
        for item in res {
            if item.hasPrefix(prefix) {
                return true
            }
        }
        return false
    }
}
```

### 解法二：

构造前缀树。

```swift
 class Trie {
    class TrieNode {
        var child: [TrieNode?] = Array.init(repeating: nil, count: 26)
        var isEnd: Bool = false
        init() {
        }
    }

    var root: TrieNode?

    /** Initialize your data structure here. */
    init() {
        root = TrieNode()
    }

    /** Inserts a word into the trie. */
    func insert(_ word: String) {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in word {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                node?.child[i] = TrieNode()
            }
            node = node?.child[i]
        }
        node?.isEnd = true
    }

    /** Returns if the word is in the trie. */
    func search(_ word: String) -> Bool {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in word {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                return false
            }
            node = node?.child[i]
        }
        return node!.isEnd
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    func startsWith(_ prefix: String) -> Bool {
        var node = root
        let startI = Int("a".unicodeScalars.first!.value)
        for c in prefix {
            let i = Int(c.unicodeScalars.first!.value)-startI
            if node?.child[i] == nil {
                return false
            }
            node = node?.child[i]
        }
        return true
    }
 }
```
