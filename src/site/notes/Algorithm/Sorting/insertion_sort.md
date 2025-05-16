---
{"dg-publish":true,"permalink":"/algorithm/sorting/insertion-sort/"}
---


# Insertion Sort

```python
def insertion_sort(arr):
    """
    Insertion Sort Algorithm
    Time Complexity: O(n²) in worst case, O(n) in best case
    Space Complexity: O(1)
    """
    # Traverse through 1 to len(arr)
    for i in range(1, len(arr)):
        key = arr[i]
        # Move elements of arr[0..i-1], that are greater than key,
        # to one position ahead of their current position
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6]
    print("Original array:", arr)
    insertion_sort(arr)
    print("Sorted array:", arr)
```

## Explanation
Insertion sort is a simple sorting algorithm that builds the final sorted array one item at a time. It is much less efficient on large lists than more advanced algorithms such as quicksort, heapsort, or merge sort.

### How it works:
1. Start with the second element (index 1) of the array
2. Compare it with the elements before it
3. Move the element to its correct position in the sorted part of the array
4. Repeat for all elements

### Time Complexity:
- Best Case: O(n) - When array is already sorted
- Average Case: O(n²)
- Worst Case: O(n²) - When array is sorted in reverse order

### Space Complexity: O(1) - In-place sorting algorithm

### Stability: Yes, insertion sort is stable
