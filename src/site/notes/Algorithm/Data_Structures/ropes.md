---
{"dg-publish":true,"permalink":"/algorithm/data-structures/ropes/"}
---


# Rope Operations

```python
class Node:
    def __init__(self, data=None, weight=0):
        """
        Initialize Rope node
        """
        self.data = data
        self.weight = weight
        self.left = None
        self.right = None
        self.parent = None

class Rope:
    def __init__(self, string=""):
        """
        Initialize Rope
        """
        self.root = None
        if string:
            self.root = self._build_rope(string)

    def _build_rope(self, string):
        """
        Build rope from string
        Time Complexity: O(n)
        """
        if len(string) <= 10:  # Leaf node threshold
            return Node(string, len(string))

        mid = len(string) // 2
        left = self._build_rope(string[:mid])
        right = self._build_rope(string[mid:])
        node = Node(None, mid)
        node.left = left
        node.right = right
        left.parent = node
        right.parent = node
        return node

    def _update_weights(self, node):
        """
        Update weights after operations
        Time Complexity: O(log n)
        """
        while node:
            if node.left:
                node.weight = self._get_weight(node.left)
            node = node.parent

    def _get_weight(self, node):
        """
        Get weight of node
        Time Complexity: O(1)
        """
        if not node:
            return 0
        if node.data:
            return len(node.data)
        return node.weight

    def _concat(self, left, right):
        """
        Concatenate two ropes
        Time Complexity: O(1)
        """
        if not left:
            return right
        if not right:
            return left

        node = Node(None, self._get_weight(left))
        node.left = left
        node.right = right
        left.parent = node
        right.parent = node
        return node

    def _split(self, node, index):
        """
        Split rope at index
        Time Complexity: O(log n)
        """
        if not node:
            return None, None

        if node.data:
            if index <= 0:
                return None, node
            if index >= len(node.data):
                return node, None
            left = Node(node.data[:index], index)
            right = Node(node.data[index:], len(node.data) - index)
            return left, right

        if index <= node.weight:
            left, right = self._split(node.left, index)
            return left, self._concat(right, node.right)
        else:
            left, right = self._split(node.right, index - node.weight)
            return self._concat(node.left, left), right

    def insert(self, string, index):
        """
        Insert string at index
        Time Complexity: O(log n)
        """
        if not self.root:
            self.root = self._build_rope(string)
            return

        left, right = self._split(self.root, index)
        new_rope = self._build_rope(string)
        self.root = self._concat(self._concat(left, new_rope), right)

    def delete(self, start, end):
        """
        Delete substring from start to end
        Time Complexity: O(log n)
        """
        if not self.root:
            return

        left, temp = self._split(self.root, start)
        middle, right = self._split(temp, end - start)
        self.root = self._concat(left, right)

    def substring(self, start, end):
        """
        Get substring from start to end
        Time Complexity: O(log n)
        """
        if not self.root:
            return ""

        left, temp = self._split(self.root, start)
        middle, right = self._split(temp, end - start)
        result = self._get_string(middle)
        self.root = self._concat(self._concat(left, middle), right)
        return result

    def _get_string(self, node):
        """
        Get string from node
        Time Complexity: O(n)
        """
        if not node:
            return ""
        if node.data:
            return node.data
        return self._get_string(node.left) + self._get_string(node.right)

    def display(self):
        """
        Display rope structure
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
            print(" " * 4 * level + f"{node.data if node.data else node.weight}")
            self._display(node.left, level + 1)

# Example usage
if __name__ == "__main__":
    rope = Rope("Hello, World!")
    
    print("Initial rope:")
    rope.display()
    print("String:", rope._get_string(rope.root))
    
    # Insert string
    rope.insert("Beautiful ", 7)
    print("\nAfter inserting 'Beautiful ' at index 7:")
    rope.display()
    print("String:", rope._get_string(rope.root))
    
    # Delete substring
    rope.delete(0, 7)
    print("\nAfter deleting first 7 characters:")
    rope.display()
    print("String:", rope._get_string(rope.root))
    
    # Get substring
    substring = rope.substring(0, 9)
    print("\nSubstring from 0 to 9:", substring)
    
    # Concatenate ropes
    rope2 = Rope(" How are you?")
    rope.root = rope._concat(rope.root, rope2.root)
    print("\nAfter concatenation:")
    rope.display()
    print("String:", rope._get_string(rope.root))
```

## Explanation
A Rope is a data structure for efficiently storing and manipulating long strings. It splits strings into smaller pieces and stores them in a balanced binary tree.

### Operations:
1. Build: Construct rope from string
2. Insert: Insert string at index
3. Delete: Delete substring
4. Substring: Get substring
5. Display: Show rope structure

### Time Complexities:
- Build: O(n)
- Insert: O(log n)
- Delete: O(log n)
- Substring: O(log n)
- Display: O(n)

### Space Complexity: O(n)

### Applications:
- Text editors
- String processing
- DNA sequencing
- Data compression
- Version control
- Document processing

### Advantages:
- Efficient string operations
- Memory efficient
- Good for large strings
- Easy to modify
- Fast concatenation

### Disadvantages:
- Complex implementation
- Overhead for small strings
- Not suitable for all problems
- Memory overhead
- Limited operations 