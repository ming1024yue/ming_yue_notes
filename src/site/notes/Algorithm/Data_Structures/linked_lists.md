---
{"dg-publish":true,"permalink":"/algorithm/data-structures/linked-lists/"}
---

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None

    def insert_at_beginning(self, data):
        """
        Insert node at the beginning
        Time Complexity: O(1)
        """
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def insert_at_end(self, data):
        """
        Insert node at the end
        Time Complexity: O(n)
        """
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        
        last = self.head
        while last.next:
            last = last.next
        last.next = new_node

    def insert_after(self, prev_node, data):
        """
        Insert node after a given node
        Time Complexity: O(1)
        """
        if not prev_node:
            print("Previous node must be in the list")
            return
        
        new_node = Node(data)
        new_node.next = prev_node.next
        prev_node.next = new_node

    def delete_node(self, key):
        """
        Delete node with given key
        Time Complexity: O(n)
        """
        temp = self.head
        
        # If head node holds the key
        if temp and temp.data == key:
            self.head = temp.next
            temp = None
            return
        
        # Search for the key
        prev = None
        while temp and temp.data != key:
            prev = temp
            temp = temp.next
        
        # If key not found
        if not temp:
            return
        
        # Unlink the node
        prev.next = temp.next
        temp = None

    def search(self, key):
        """
        Search for a key in the list
        Time Complexity: O(n)
        """
        current = self.head
        while current:
            if current.data == key:
                return True
            current = current.next
        return False

    def print_list(self):
        """
        Print the linked list
        Time Complexity: O(n)
        """
        temp = self.head
        while temp:
            print(temp.data, end=" -> ")
            temp = temp.next
        print("None")

# Example usage
if __name__ == "__main__":
    llist = LinkedList()
    
    # Insert elements
    llist.insert_at_end(1)
    llist.insert_at_beginning(2)
    llist.insert_at_end(4)
    llist.insert_after(llist.head.next, 3)
    
    print("Created linked list:")
    llist.print_list()
    
    # Search for element
    print("Is 3 in the list?", llist.search(3))
    
    # Delete element
    llist.delete_node(3)
    print("After deleting 3:")
    llist.print_list()
```

## Explanation
A linked list is a linear data structure where elements are stored in nodes, and each node points to the next node in the sequence. This implementation provides basic operations on singly linked lists.

### Operations:
1. Insert at beginning: Add a node at the start of the list
2. Insert at end: Add a node at the end of the list
3. Insert after: Add a node after a given node
4. Delete: Remove a node with given data
5. Search: Find if a value exists in the list
6. Print: Display the list

### Time Complexities:
- Insert at beginning: O(1)
- Insert at end: O(n)
- Insert after: O(1)
- Delete: O(n)
- Search: O(n)
- Print: O(n)

### Space Complexity: O(n)

### Applications:
- Implementing stacks and queues
- Hash table collision handling
- Memory management
- Polynomial representation
- Graph representation

### Advantages:
- Dynamic size
- Easy insertion and deletion
- No memory waste
- No need for contiguous memory

### Disadvantages:
- No random access
- Extra memory for pointers
- Not cache friendly
- More complex implementation
