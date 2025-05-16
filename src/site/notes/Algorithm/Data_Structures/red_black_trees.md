---
{"dg-publish":true,"permalink":"/algorithm/data-structures/red-black-trees/"}
---

```python
class Node:
    def __init__(self, key, color="RED"):
        """
        Initialize Red-Black tree node
        """
        self.key = key
        self.color = color
        self.left = None
        self.right = None
        self.parent = None

class RedBlackTree:
    def __init__(self):
        """
        Initialize Red-Black tree
        """
        self.NIL = Node(None, "BLACK")
        self.root = self.NIL

    def left_rotate(self, x):
        """
        Left rotate subtree rooted at x
        Time Complexity: O(1)
        """
        y = x.right
        x.right = y.left
        if y.left != self.NIL:
            y.left.parent = x
        y.parent = x.parent
        if x.parent == self.NIL:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y

    def right_rotate(self, x):
        """
        Right rotate subtree rooted at x
        Time Complexity: O(1)
        """
        y = x.left
        x.left = y.right
        if y.right != self.NIL:
            y.right.parent = x
        y.parent = x.parent
        if x.parent == self.NIL:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y

    def insert(self, key):
        """
        Insert key into tree
        Time Complexity: O(log n)
        """
        z = Node(key)
        y = self.NIL
        x = self.root

        while x != self.NIL:
            y = x
            if z.key < x.key:
                x = x.left
            else:
                x = x.right

        z.parent = y
        if y == self.NIL:
            self.root = z
        elif z.key < y.key:
            y.left = z
        else:
            y.right = z

        z.left = self.NIL
        z.right = self.NIL
        z.color = "RED"
        self._insert_fixup(z)

    def _insert_fixup(self, z):
        """
        Fix Red-Black tree properties after insertion
        """
        while z.parent.color == "RED":
            if z.parent == z.parent.parent.left:
                y = z.parent.parent.right
                if y.color == "RED":
                    z.parent.color = "BLACK"
                    y.color = "BLACK"
                    z.parent.parent.color = "RED"
                    z = z.parent.parent
                else:
                    if z == z.parent.right:
                        z = z.parent
                        self.left_rotate(z)
                    z.parent.color = "BLACK"
                    z.parent.parent.color = "RED"
                    self.right_rotate(z.parent.parent)
            else:
                y = z.parent.parent.left
                if y.color == "RED":
                    z.parent.color = "BLACK"
                    y.color = "BLACK"
                    z.parent.parent.color = "RED"
                    z = z.parent.parent
                else:
                    if z == z.parent.left:
                        z = z.parent
                        self.right_rotate(z)
                    z.parent.color = "BLACK"
                    z.parent.parent.color = "RED"
                    self.left_rotate(z.parent.parent)
        self.root.color = "BLACK"

    def delete(self, key):
        """
        Delete key from tree
        Time Complexity: O(log n)
        """
        z = self.search(key)
        if z == self.NIL:
            return

        y = z
        y_original_color = y.color

        if z.left == self.NIL:
            x = z.right
            self._transplant(z, z.right)
        elif z.right == self.NIL:
            x = z.left
            self._transplant(z, z.left)
        else:
            y = self._minimum(z.right)
            y_original_color = y.color
            x = y.right
            if y.parent == z:
                x.parent = y
            else:
                self._transplant(y, y.right)
                y.right = z.right
                y.right.parent = y
            self._transplant(z, y)
            y.left = z.left
            y.left.parent = y
            y.color = z.color

        if y_original_color == "BLACK":
            self._delete_fixup(x)

    def _transplant(self, u, v):
        """
        Replace subtree rooted at u with subtree rooted at v
        """
        if u.parent == self.NIL:
            self.root = v
        elif u == u.parent.left:
            u.parent.left = v
        else:
            u.parent.right = v
        v.parent = u.parent

    def _minimum(self, x):
        """
        Find minimum node in subtree rooted at x
        """
        while x.left != self.NIL:
            x = x.left
        return x

    def _delete_fixup(self, x):
        """
        Fix Red-Black tree properties after deletion
        """
        while x != self.root and x.color == "BLACK":
            if x == x.parent.left:
                w = x.parent.right
                if w.color == "RED":
                    w.color = "BLACK"
                    x.parent.color = "RED"
                    self.left_rotate(x.parent)
                    w = x.parent.right
                if w.left.color == "BLACK" and w.right.color == "BLACK":
                    w.color = "RED"
                    x = x.parent
                else:
                    if w.right.color == "BLACK":
                        w.left.color = "BLACK"
                        w.color = "RED"
                        self.right_rotate(w)
                        w = x.parent.right
                    w.color = x.parent.color
                    x.parent.color = "BLACK"
                    w.right.color = "BLACK"
                    self.left_rotate(x.parent)
                    x = self.root
            else:
                w = x.parent.left
                if w.color == "RED":
                    w.color = "BLACK"
                    x.parent.color = "RED"
                    self.right_rotate(x.parent)
                    w = x.parent.left
                if w.right.color == "BLACK" and w.left.color == "BLACK":
                    w.color = "RED"
                    x = x.parent
                else:
                    if w.left.color == "BLACK":
                        w.right.color = "BLACK"
                        w.color = "RED"
                        self.left_rotate(w)
                        w = x.parent.left
                    w.color = x.parent.color
                    x.parent.color = "BLACK"
                    w.left.color = "BLACK"
                    self.right_rotate(x.parent)
                    x = self.root
        x.color = "BLACK"

    def search(self, key):
        """
        Search for key in tree
        Time Complexity: O(log n)
        """
        x = self.root
        while x != self.NIL and key != x.key:
            if key < x.key:
                x = x.left
            else:
                x = x.right
        return x

    def display(self):
        """
        Display tree structure
        Time Complexity: O(n)
        """
        if self.root != self.NIL:
            self._display(self.root)

    def _display(self, node, level=0):
        """
        Helper function for display
        """
        if node != self.NIL:
            self._display(node.right, level + 1)
            print(" " * 4 * level + f"{node.key}({node.color})")
            self._display(node.left, level + 1)

# Example usage
if __name__ == "__main__":
    rbt = RedBlackTree()
    
    # Insert keys
    keys = [7, 3, 18, 10, 22, 8, 11, 26]
    for key in keys:
        rbt.insert(key)
    
    print("Red-Black tree after insertion:")
    rbt.display()
    
    # Search for keys
    search_keys = [10, 15, 22]
    for key in search_keys:
        result = rbt.search(key)
        print(f"Search {key}:", "Found" if result != rbt.NIL else "Not found")
    
    # Delete keys
    delete_keys = [18, 7, 11]
    for key in delete_keys:
        rbt.delete(key)
        print(f"After deleting {key}:")
        rbt.display()
```

## Explanation
A Red-Black tree is a self-balancing binary search tree that maintains balance through node coloring and rotations. It ensures that the tree remains approximately balanced, providing efficient operations.

### Operations:
1. Insert: Add key to tree
2. Delete: Remove key from tree
3. Search: Find key in tree
4. Display: Show tree structure

### Time Complexities:
- Insert: O(log n)
- Delete: O(log n)
- Search: O(log n)
- Display: O(n)

### Space Complexity: O(n)

### Applications:
- Database indexing
- Memory management
- Network routing
- File systems
- Compiler design
- Symbol tables

### Advantages:
- Guaranteed O(log n) operations
- Memory efficient
- Self-balancing
- Suitable for large datasets
- Good for dynamic data

### Disadvantages:
- Complex implementation
- Overhead for small datasets
- Not suitable for all problems
- Memory overhead
- Limited operations
