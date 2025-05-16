---
{"dg-publish":true,"permalink":"/algorithm/data-structures/treaps/"}
---

```python
import random

class Node:
    def __init__(self, key, priority):
        """
        Initialize Treap node
        """
        self.key = key
        self.priority = priority
        self.left = None
        self.right = None
        self.parent = None

class Treap:
    def __init__(self):
        """
        Initialize Treap
        """
        self.root = None

    def _left_rotate(self, x):
        """
        Left rotate subtree rooted at x
        Time Complexity: O(1)
        """
        y = x.right
        x.right = y.left
        if y.left:
            y.left.parent = x
        y.parent = x.parent
        if not x.parent:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y

    def _right_rotate(self, x):
        """
        Right rotate subtree rooted at x
        Time Complexity: O(1)
        """
        y = x.left
        x.left = y.right
        if y.right:
            y.right.parent = x
        y.parent = x.parent
        if not x.parent:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y

    def insert(self, key):
        """
        Insert key into treap
        Time Complexity: O(log n) average case
        """
        priority = random.random()
        node = Node(key, priority)
        
        if not self.root:
            self.root = node
            return

        current = self.root
        parent = None

        while current:
            parent = current
            if key < current.key:
                current = current.left
            else:
                current = current.right

        node.parent = parent
        if key < parent.key:
            parent.left = node
        else:
            parent.right = node

        self._heapify(node)

    def _heapify(self, node):
        """
        Maintain heap property after insertion
        Time Complexity: O(log n) average case
        """
        while node.parent and node.priority > node.parent.priority:
            if node == node.parent.left:
                self._right_rotate(node.parent)
            else:
                self._left_rotate(node.parent)

    def delete(self, key):
        """
        Delete key from treap
        Time Complexity: O(log n) average case
        """
        node = self._search(key)
        if not node:
            return

        while node.left or node.right:
            if not node.left:
                self._left_rotate(node)
            elif not node.right:
                self._right_rotate(node)
            elif node.left.priority > node.right.priority:
                self._right_rotate(node)
            else:
                self._left_rotate(node)

        if node.parent:
            if node == node.parent.left:
                node.parent.left = None
            else:
                node.parent.right = None
        else:
            self.root = None

    def search(self, key):
        """
        Search for key in treap
        Time Complexity: O(log n) average case
        """
        return self._search(key)

    def _search(self, key):
        """
        Helper function for search
        """
        current = self.root
        while current:
            if key == current.key:
                return current
            elif key < current.key:
                current = current.left
            else:
                current = current.right
        return None

    def display(self):
        """
        Display treap structure
        Time Complexity: O(n)
        """
        if self.root:
            self._display(self.root)

    def _display(self, node, level=0):
        """
        Helper function for display
        """
        if node:
            self._display(node.right, level + 1)
            print(" " * 4 * level + f"{node.key}({node.priority:.2f})")
            self._display(node.left, level + 1)

# Example usage
if __name__ == "__main__":
    treap = Treap()
    
    # Insert keys
    keys = [50, 30, 20, 40, 70, 60, 80]
    for key in keys:
        treap.insert(key)
    
    print("Treap after insertion:")
    treap.display()
    
    # Search for keys
    search_keys = [30, 60, 90]
    for key in search_keys:
        result = treap.search(key)
        print(f"Search {key}:", "Found" if result else "Not found")
    
    # Delete keys
    delete_keys = [30, 70]
    for key in delete_keys:
        treap.delete(key)
        print(f"After deleting {key}:")
        treap.display()
```

## Explanation
A Treap is a binary search tree that maintains both binary search tree and heap properties. Each node has a key and a random priority, where keys follow BST order and priorities follow heap order.

### Operations:
1. Insert: Add key to treap
2. Delete: Remove key from treap
3. Search: Find key in treap
4. Display: Show treap structure

### Time Complexities:
- Insert: O(log n) average case
- Delete: O(log n) average case
- Search: O(log n) average case
- Display: O(n)

### Space Complexity: O(n)

### Applications:
- Randomized search trees
- Priority queues
- Dictionary implementation
- Range queries
- Graph algorithms
- Memory management

### Advantages:
- Simple implementation
- Expected O(log n) operations
- Memory efficient
- Self-balancing
- Good for dynamic data

### Disadvantages:
- Not guaranteed O(log n) worst case
- Random priorities
- Not suitable for all problems
- Memory overhead
- Limited operations 