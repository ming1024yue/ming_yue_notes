---
{"dg-publish":true,"permalink":"/algorithm/data-structures/splay-trees/"}
---

```python
class Node:
    def __init__(self, key):
        """
        Initialize Splay tree node
        """
        self.key = key
        self.left = None
        self.right = None
        self.parent = None

class SplayTree:
    def __init__(self):
        """
        Initialize Splay tree
        """
        self.root = None

    def _zig(self, x):
        """
        Right rotation
        Time Complexity: O(1)
        """
        y = x.left
        x.left = y.right
        if y.right:
            y.right.parent = x
        y.parent = x.parent
        if not x.parent:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.right = x
        x.parent = y

    def _zag(self, x):
        """
        Left rotation
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

    def _splay(self, x):
        """
        Splay operation to move node to root
        Time Complexity: O(log n) amortized
        """
        while x.parent:
            if not x.parent.parent:
                if x == x.parent.left:
                    self._zig(x.parent)
                else:
                    self._zag(x.parent)
            elif x == x.parent.left and x.parent == x.parent.parent.left:
                self._zig(x.parent.parent)
                self._zig(x.parent)
            elif x == x.parent.right and x.parent == x.parent.parent.right:
                self._zag(x.parent.parent)
                self._zag(x.parent)
            elif x == x.parent.right and x.parent == x.parent.parent.left:
                self._zag(x.parent)
                self._zig(x.parent)
            else:
                self._zig(x.parent)
                self._zag(x.parent)

    def insert(self, key):
        """
        Insert key into tree
        Time Complexity: O(log n) amortized
        """
        node = Node(key)
        y = None
        x = self.root

        while x:
            y = x
            if node.key < x.key:
                x = x.left
            else:
                x = x.right

        node.parent = y
        if not y:
            self.root = node
        elif node.key < y.key:
            y.left = node
        else:
            y.right = node

        self._splay(node)

    def _search(self, key):
        """
        Search for key in tree
        Time Complexity: O(log n) amortized
        """
        x = self.root
        while x:
            if key < x.key:
                x = x.left
            elif key > x.key:
                x = x.right
            else:
                return x
        return None

    def search(self, key):
        """
        Search for key and splay
        Time Complexity: O(log n) amortized
        """
        x = self._search(key)
        if x:
            self._splay(x)
        return x

    def delete(self, key):
        """
        Delete key from tree
        Time Complexity: O(log n) amortized
        """
        node = self._search(key)
        if not node:
            return

        self._splay(node)

        if not node.left:
            self.root = node.right
            if self.root:
                self.root.parent = None
        elif not node.right:
            self.root = node.left
            if self.root:
                self.root.parent = None
        else:
            x = node.left
            while x.right:
                x = x.right
            self._splay(x)
            x.right = node.right
            if node.right:
                node.right.parent = x
            self.root = x
            self.root.parent = None

    def display(self):
        """
        Display tree structure
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
            print(" " * 4 * level + str(node.key))
            self._display(node.left, level + 1)

# Example usage
if __name__ == "__main__":
    st = SplayTree()
    
    # Insert keys
    keys = [100, 50, 200, 40, 30, 20, 25]
    for key in keys:
        st.insert(key)
    
    print("Splay tree after insertion:")
    st.display()
    
    # Search for keys
    search_keys = [50, 25, 300]
    for key in search_keys:
        result = st.search(key)
        print(f"Search {key}:", "Found" if result else "Not found")
        print("Tree after search:")
        st.display()
    
    # Delete keys
    delete_keys = [50, 200]
    for key in delete_keys:
        st.delete(key)
        print(f"After deleting {key}:")
        st.display()
```

## Explanation
A Splay tree is a self-adjusting binary search tree that moves recently accessed elements to the root through a splaying operation. It provides good performance for frequently accessed elements.

### Operations:
1. Insert: Add key to tree
2. Delete: Remove key from tree
3. Search: Find key in tree
4. Display: Show tree structure

### Time Complexities:
- Insert: O(log n) amortized
- Delete: O(log n) amortized
- Search: O(log n) amortized
- Display: O(n)

### Space Complexity: O(n)

### Applications:
- Cache implementation
- Memory management
- Network routing
- File systems
- Compiler design
- Symbol tables

### Advantages:
- Self-adjusting
- Good for frequently accessed elements
- Simple implementation
- Memory efficient
- No extra storage needed

### Disadvantages:
- Not suitable for all problems
- Can be slow for some operations
- Not guaranteed O(log n) worst case
- Memory overhead
- Limited operations 