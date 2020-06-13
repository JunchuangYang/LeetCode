# [208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

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

**[题解](<https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/biao-zhun-de-zi-dian-shu-xiang-jie-by-tunsuy/>)**

```python
class TrieNode(object):
    def __init__(self):
        # 当前节点的子树节点
        self.children = defaultdict(TrieNode)
        # 从根节点到该节点的路径是否一个单词
        self.is_word = False

class Trie(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # 字典树的根节点
        self.root = TrieNode()


    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: None
        """
        # 从根节点开始遍历
        cur_node = self.root
        # 每个字符是一个节点，如果没有则新建，有则直接向下遍历继续插入
        for ch in word:
            cur_node = cur_node.children[ch]
        # 整个单词中的每个字符都对应有一个节点了，并且是通过children关联起来的，
        # 那么这最后一个字符的节点就表示一个单词的结尾
        cur_node.is_word = True


    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        # 从根节点向下遍历
        cur_node = self.root
        for ch in word:
            cur_node = cur_node.children.get(ch)
            # 如果某个字符没有对应的节点，说明其对应的单词没有在该字典树中
            if not cur_node:
                return False
        # 该单词所有的字符均有对应的节点，那么判断最后一个字符是否是字典中一个单词的结尾
        return cur_node.is_word


    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        # 从根节点向下遍历
        cur_node = self.root
        for ch in prefix:
            # 如果某个字符没有对应的节点，说明其对应的单词没有在该字典树中
            cur_node = cur_node.children.get(ch)
            if not cur_node:
                return False
        # 该单词所有的字符均有对应的节点，那么该单词一个是字典树中某些单词的前缀
        return True



# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

