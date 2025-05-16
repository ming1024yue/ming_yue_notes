---
{"dg-publish":true,"permalink":"/algorithm/data-structures/arrays/"}
---


# Array Operations

```python
class Array:
    def __init__(self, size):
        """
        Initialize array with given size
        """
        self.size = size
        self.arr = [None] * size
        self.length = 0

    def insert(self, index, value):
        """
        Insert value at given index
        Time Complexity: O(n)
        """
        if index < 0 or index > self.length:
            raise IndexError("Index out of bounds")
        
        if self.length == self.size:
            raise Exception("Array is full")
        
        # Shift elements to make space
        for i in range(self.length, index, -1):
            self.arr[i] = self.arr[i - 1]
        
        self.arr[index] = value
        self.length += 1

    def delete(self, index):
        """
        Delete value at given index
        Time Complexity: O(n)
        """
        if index < 0 or index >= self.length:
            raise IndexError("Index out of bounds")
        
        # Shift elements to fill the gap
        for i in range(index, self.length - 1):
            self.arr[i] = self.arr[i + 1]
        
        self.arr[self.length - 1] = None
        self.length -= 1

    def search(self, value):
        """
        Search for value in array
        Time Complexity: O(n)
        """
        for i in range(self.length):
            if self.arr[i] == value:
                return i
        return -1

    def get(self, index):
        """
        Get value at given index
        Time Complexity: O(1)
        """
        if index < 0 or index >= self.length:
            raise IndexError("Index out of bounds")
        return self.arr[index]

    def update(self, index, value):
        """
        Update value at given index
        Time Complexity: O(1)
        """
        if index < 0 or index >= self.length:
            raise IndexError("Index out of bounds")
        self.arr[index] = value

# Example usage
if __name__ == "__main__":
    arr = Array(5)
    
    # Insert elements
    arr.insert(0, 10)
    arr.insert(1, 20)
    arr.insert(2, 30)
    print("After insertions:", arr.arr[:arr.length])
    
    # Update element
    arr.update(1, 25)
    print("After update:", arr.arr[:arr.length])
    
    # Search element
    index = arr.search(30)
    print("Index of 30:", index)
    
    # Delete element
    arr.delete(1)
    print("After deletion:", arr.arr[:arr.length])
```

## Explanation
Arrays are one of the most fundamental data structures in computer science. This implementation provides basic operations on arrays with their time complexities.

### Operations:
1. Insert: Add an element at a specific index
2. Delete: Remove an element from a specific index
3. Search: Find the index of a specific value
4. Get: Retrieve the value at a specific index
5. Update: Change the value at a specific index

### Time Complexities:
- Insert: O(n) - May need to shift elements
- Delete: O(n) - May need to shift elements
- Search: O(n) - Linear search
- Get: O(1) - Direct access
- Update: O(1) - Direct access

### Space Complexity: O(n)

### Applications:
- Dynamic programming
- Sorting algorithms
- Searching algorithms
- Matrix operations
- Cache implementation

### Advantages:
- Fast random access
- Simple implementation
- Memory efficient
- Cache friendly

### Disadvantages:
- Fixed size
- Slow insertions and deletions
- Memory waste if not fully utilized
- Costly resizing
