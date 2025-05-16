---
{"dg-publish":true,"permalink":"/algorithm/data-structures/tries/"}
---

```python
class TrieNode:
    def __init__(self):
        """
        Initialize trie node
        """
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        """
        Initialize empty trie
        """
        self.root = TrieNode()

    def insert(self, word):
        """
        Insert word into trie
        Time Complexity: O(m) where m is length of word
        """
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        """
        Search for word in trie
        Time Complexity: O(m) where m is length of word
        """
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

    def starts_with(self, prefix):
        """
        Check if any word starts with prefix
        Time Complexity: O(m) where m is length of prefix
        """
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True

    def delete(self, word):
        """
        Delete word from trie
        Time Complexity: O(m) where m is length of word
        """
        def _delete_helper(node, word, index):
            if index == len(word):
                if not node.is_end_of_word:
                    return False
                node.is_end_of_word = False
                return len(node.children) == 0

            char = word[index]
            if char not in node.children:
                return False

            should_delete_child = _delete_helper(node.children[char], word, index + 1)

            if should_delete_child:
                del node.children[char]
                return len(node.children) == 0

            return False

        _delete_helper(self.root, word, 0)

    def get_all_words(self):
        """
        Get all words in trie
        Time Complexity: O(n) where n is total number of nodes
        """
        words = []
        def _collect_words(node, current_word):
            if node.is_end_of_word:
                words.append(current_word)
            for char, child in node.children.items():
                _collect_words(child, current_word + char)

        _collect_words(self.root, "")
        return words

# Example usage
if __name__ == "__main__":
    t = Trie()
    
    # Insert words
    words = ["hello", "world", "help", "he", "her", "hero"]
    for word in words:
        t.insert(word)
    
    # Search words
    print("Search 'hello':", t.search("hello"))
    print("Search 'hero':", t.search("hero"))
    print("Search 'heroes':", t.search("heroes"))
    
    # Check prefixes
    print("Starts with 'he':", t.starts_with("he"))
    print("Starts with 'wor':", t.starts_with("wor"))
    print("Starts with 'xyz':", t.starts_with("xyz"))
    
    # Get all words
    print("All words:", t.get_all_words())
    
    # Delete word
    t.delete("hero")
    print("After deleting 'hero':", t.get_all_words())
```

## Explanation
A trie (prefix tree) is a tree-like data structure that stores strings. Each node represents a character, and the path from root to any node represents a prefix of some string.

### Operations:
1. Insert: Add new word
2. Search: Check if word exists
3. Starts With: Check if prefix exists
4. Delete: Remove word
5. Get All Words: Retrieve all stored words

### Time Complexities:
- Insert: O(m) where m is word length
- Search: O(m) where m is word length
- Starts With: O(m) where m is prefix length
- Delete: O(m) where m is word length
- Get All Words: O(n) where n is total nodes

### Space Complexity: O(n * m) where n is number of words and m is average word length

### Applications:
- Autocomplete
- Spell checking
- IP routing
- Word games
- Text search
- Dictionary implementation

### Advantages:
- Fast prefix search
- Memory efficient for common prefixes
- Flexible
- Easy to implement
- Useful for string operations

### Disadvantages:
- Memory intensive
- Complex implementation
- Not suitable for all problems
- Can be slow for some operations
- Space overhead 