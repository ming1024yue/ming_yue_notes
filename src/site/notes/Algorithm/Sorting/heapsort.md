---
{"dg-publish":true,"permalink":"/algorithm/sorting/heapsort/"}
---


# Heap Sort

```python
def heapify(arr, n, i):
    """
    To heapify subtree rooted at index i.
    n is size of heap
    """
    largest = i  # Initialize largest as root
    left = 2 * i + 1
    right = 2 * i + 2

    # If left child is larger than root
    if left < n and arr[i] < arr[left]:
        largest = left

    # If right child is larger than largest so far
    if right < n and arr[largest] < arr[right]:
        largest = right

    # If largest is not root
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        # Recursively heapify the affected sub-tree
        heapify(arr, n, largest)

def heap_sort(arr):
    """
    Heapsort Algorithm
    Time Complexity: O(n log n)
    Space Complexity: O(1)
    """
    n = len(arr)

    # Build a maxheap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # One by one extract elements
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]  # swap
        heapify(arr, i, 0)

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6, 7]
    print("Original array:", arr)
    heap_sort(arr)
    print("Sorted array:", arr)
```

## Explanation
Heapsort is a comparison-based sorting algorithm that uses a binary heap data structure. It's similar to selection sort where we first find the maximum element and place it at the end.

### How it works:
1. Build a max heap from the input data
2. The largest item is stored at the root of the heap
3. Replace it with the last item of the heap followed by reducing the size of heap by 1
4. Heapify the root of the tree
5. Repeat while size of heap is greater than 1

### Time Complexity:
- Best Case: O(n log n)
- Average Case: O(n log n)
- Worst Case: O(n log n)

### Space Complexity: O(1) - In-place sorting algorithm

### Stability: No, heapsort is not stable

### Advantages:
- Guaranteed O(n log n) performance
- In-place sorting
- No extra space required

### Disadvantages:
- Not stable
- More complex than simpler algorithms
- Poor cache performance
