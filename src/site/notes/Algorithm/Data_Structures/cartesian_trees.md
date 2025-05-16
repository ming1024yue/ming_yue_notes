---
{"dg-publish":true,"permalink":"/algorithm/data-structures/cartesian-trees/"}
---


# Cartesian Tree Operations

```python
class Node:
    def __init__(self, key, value):
        """
        Initialize Cartesian tree node
        """
        self.key = key
        self.value = value
        self.left = None
        self.right = None
        self.parent = None

class CartesianTree:
    def __init__(self):
        """
        Initialize Cartesian tree
        """
        self.root = None

    def build(self, keys, values):
        """
        Build Cartesian tree from arrays
        Time Complexity: O(n)
        """
        if not keys or not values or len(keys) != len(values):
            return None

        n = len(keys)
        parent = [None] * n
        left = [None] * n
        right = [None] * n

        # Build tree using stack
        stack = []
        for i in range(n):
            last = None
            while stack and values[stack[-1]] > values[i]:
                last = stack.pop()

            if stack:
                parent[i] = stack[-1]
                right[stack[-1]] = i
            else:
                self.root = i

            if last:
                parent[last] = i
                left[i] = last

            stack.append(i)

        # Convert to tree structure
        nodes = [Node(keys[i], values[i]) for i in range(n)]
        for i in range(n):
            if parent[i] is not None:
                nodes[i].parent = nodes[parent[i]]
            if left[i] is not None:
                nodes[i].left = nodes[left[i]]
            if right[i] is not None:
                nodes[i].right = nodes[right[i]]

        self.root = nodes[self.root] if self.root is not None else None

    def insert(self, key, value):
        """
        Insert key-value pair into tree
        Time Complexity: O(n)
        """
        node = Node(key, value)
        if not self.root:
            self.root = node
            return

        current = self.root
        parent = None

        while current and current.value <= value:
            parent = current
            current = current.right

        if not parent:
            node.left = self.root
            self.root.parent = node
            self.root = node
        else:
            node.left = parent.right
            if parent.right:
                parent.right.parent = node
            parent.right = node
            node.parent = parent

    def delete(self, key):
        """
        Delete node with given key
        Time Complexity: O(n)
        """
        node = self._search(key)
        if not node:
            return

        if not node.left and not node.right:
            if not node.parent:
                self.root = None
            elif node == node.parent.left:
                node.parent.left = None
            else:
                node.parent.right = None
        elif not node.left:
            if not node.parent:
                self.root = node.right
                node.right.parent = None
            elif node == node.parent.left:
                node.parent.left = node.right
                node.right.parent = node.parent
            else:
                node.parent.right = node.right
                node.right.parent = node.parent
        elif not node.right:
            if not node.parent:
                self.root = node.left
                node.left.parent = None
            elif node == node.parent.left:
                node.parent.left = node.left
                node.left.parent = node.parent
            else:
                node.parent.right = node.left
                node.left.parent = node.parent
        else:
            successor = self._minimum(node.right)
            node.key = successor.key
            node.value = successor.value
            if successor.parent.left == successor:
                successor.parent.left = successor.right
            else:
                successor.parent.right = successor.right
            if successor.right:
                successor.right.parent = successor.parent

    def _minimum(self, node):
        """
        Find minimum node in subtree
        Time Complexity: O(log n)
        """
        while node.left:
            node = node.left
        return node

    def search(self, key):
        """
        Search for key in tree
        Time Complexity: O(n)
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
            print(" " * 4 * level + f"{node.key}({node.value})")
            self._display(node.left, level + 1)

# Example usage
if __name__ == "__main__":
    ct = CartesianTree()
    
    # Build tree from arrays
    keys = [9, 3, 7, 1, 8, 12, 10, 20, 15, 18, 5]
    values = [5, 3, 4, 1, 2, 6, 7, 8, 9, 10, 11]
    ct.build(keys, values)
    
    print("Cartesian tree after building:")
    ct.display()
    
    # Insert key-value pairs
    pairs = [(2, 0), (4, 1), (6, 2)]
    for key, value in pairs:
        ct.insert(key, value)
        print(f"After inserting ({key}, {value}):")
        ct.display()
    
    # Search for keys
    search_keys = [7, 15, 25]
    for key in search_keys:
        result = ct.search(key)
        print(f"Search {key}:", "Found" if result else "Not found")
    
    # Delete keys
    delete_keys = [7, 12, 5]
    for key in delete_keys:
        ct.delete(key)
        print(f"After deleting {key}:")
        ct.display()
```

## Explanation
A Cartesian tree is a binary tree derived from a sequence of numbers. It maintains heap property with respect to values and binary search tree property with respect to keys.

### Operations:
1. Build: Construct tree from arrays
2. Insert: Add key-value pair
3. Delete: Remove node by key
4. Search: Find node by key
5. Display: Show tree structure

### Time Complexities:
- Build: O(n)
- Insert: O(n)
- Delete: O(n)
- Search: O(n)
- Display: O(n)

### Space Complexity: O(n)

### Applications:
- Range minimum queries
- Sorting algorithms
- Priority queues
- Data compression
- Graph algorithms
- Memory management

### Advantages:
- Simple implementation
- Memory efficient
- Good for range queries
- Useful for sorting
- Easy to understand

### Disadvantages:
- Not self-balancing
- Can be unbalanced
- Not suitable for all problems
- Memory overhead
- Limited operations 