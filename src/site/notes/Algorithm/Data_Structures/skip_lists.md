---
{"dg-publish":true,"permalink":"/algorithm/data-structures/skip-lists/"}
---


```python
import random

class Node:
    def __init__(self, key, value, level):
        """
        Initialize skip list node
        """
        self.key = key
        self.value = value
        self.forward = [None] * (level + 1)

class SkipList:
    def __init__(self, max_level=16, p=0.5):
        """
        Initialize skip list
        max_level: maximum level of skip list
        p: probability of node promotion
        """
        self.max_level = max_level
        self.p = p
        self.level = 0
        self.header = Node(None, None, max_level)

    def _random_level(self):
        """
        Generate random level for node
        """
        level = 0
        while random.random() < self.p and level < self.max_level:
            level += 1
        return level

    def insert(self, key, value):
        """
        Insert key-value pair
        Time Complexity: O(log n) average case
        """
        update = [None] * (self.max_level + 1)
        current = self.header

        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]
            update[i] = current

        current = current.forward[0]

        if current and current.key == key:
            current.value = value
        else:
            new_level = self._random_level()
            if new_level > self.level:
                for i in range(self.level + 1, new_level + 1):
                    update[i] = self.header
                self.level = new_level

            new_node = Node(key, value, new_level)
            for i in range(new_level + 1):
                new_node.forward[i] = update[i].forward[i]
                update[i].forward[i] = new_node

    def search(self, key):
        """
        Search for key
        Time Complexity: O(log n) average case
        """
        current = self.header
        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]

        current = current.forward[0]
        if current and current.key == key:
            return current.value
        return None

    def delete(self, key):
        """
        Delete key-value pair
        Time Complexity: O(log n) average case
        """
        update = [None] * (self.max_level + 1)
        current = self.header

        for i in range(self.level, -1, -1):
            while current.forward[i] and current.forward[i].key < key:
                current = current.forward[i]
            update[i] = current

        current = current.forward[0]

        if current and current.key == key:
            for i in range(self.level + 1):
                if update[i].forward[i] != current:
                    break
                update[i].forward[i] = current.forward[i]

            while self.level > 0 and self.header.forward[self.level] is None:
                self.level -= 1

            return True
        return False

    def display(self):
        """
        Display skip list
        Time Complexity: O(n)
        """
        print("Skip List:")
        for i in range(self.level + 1):
            print(f"Level {i}: ", end="")
            node = self.header.forward[i]
            while node:
                print(f"({node.key}: {node.value})", end=" -> ")
                node = node.forward[i]
            print("None")

# Example usage
if __name__ == "__main__":
    sl = SkipList()
    
    # Insert key-value pairs
    pairs = [(3, "three"), (6, "six"), (7, "seven"), (9, "nine"),
             (12, "twelve"), (19, "nineteen"), (17, "seventeen"),
             (26, "twenty six"), (21, "twenty one"), (25, "twenty five")]
    
    for key, value in pairs:
        sl.insert(key, value)
    
    # Display skip list
    sl.display()
    
    # Search for keys
    keys = [3, 7, 12, 25, 30]
    for key in keys:
        value = sl.search(key)
        print(f"Search {key}:", value if value else "Not found")
    
    # Delete keys
    keys = [6, 19, 25]
    for key in keys:
        success = sl.delete(key)
        print(f"Delete {key}:", "Success" if success else "Failed")
    
    # Display updated skip list
    sl.display()
```

## Explanation
A skip list is a probabilistic data structure that allows for efficient search, insertion, and deletion operations. It maintains a linked hierarchy of subsequences, with each successive subsequence skipping over fewer elements.

### Operations:
1. Insert: Add key-value pair
2. Search: Find value by key
3. Delete: Remove key-value pair
4. Display: Show skip list structure

### Time Complexities:
- Insert: O(log n) average case
- Search: O(log n) average case
- Delete: O(log n) average case
- Display: O(n)

### Space Complexity: O(n log n) average case

### Applications:
- Database indexing
- Priority queues
- Dictionary implementation
- Range queries
- Graph algorithms
- Memory management

### Advantages:
- Simple implementation
- Fast operations
- Memory efficient
- Easy to understand
- Useful for many problems

### Disadvantages:
- Probabilistic structure
- Not suitable for all problems
- Can be slow for some operations
- Memory overhead
- Limited operations 