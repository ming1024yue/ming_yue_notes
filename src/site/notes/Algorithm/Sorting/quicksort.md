---
{"dg-publish":true,"permalink":"/algorithm/sorting/quicksort/"}
---


```python

def partition(arr, low, high):
    """
    This function takes last element as pivot, places
    the pivot element at its correct position in sorted
    array, and places all smaller (smaller than pivot)
    to left of pivot and all greater elements to right
    of pivot
    """
    i = (low - 1)  # index of smaller element
    pivot = arr[high]  # pivot

    for j in range(low, high):
        # If current element is smaller than or equal to pivot
        if arr[j] <= pivot:
            # increment index of smaller element
            i = i + 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return (i + 1)

def quick_sort(arr, low, high):
    """
    Quicksort Algorithm
    Time Complexity: O(n log n) average case, O(n²) worst case
    Space Complexity: O(log n)
    """
    if len(arr) == 1:
        return arr
    if low < high:
        # pi is partitioning index, arr[p] is now at right place
        pi = partition(arr, low, high)

        # Separately sort elements before partition and after partition
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

# Example usage
if __name__ == "__main__":
    arr = [10, 7, 8, 9, 1, 5]
    n = len(arr)
    print("Original array:", arr)
    quick_sort(arr, 0, n - 1)
    print("Sorted array:", arr)
```

## Explanation
Quicksort is a divide-and-conquer algorithm. It works by selecting a 'pivot' element from the array and partitioning the other elements into two sub-arrays, according to whether they are less than or greater than the pivot.

### How it works:
1. Pick an element, called a pivot, from the array
2. Partitioning: reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it
3. Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values

### Time Complexity:
- Best Case: O(n log n)
- Average Case: O(n log n)
- Worst Case: O(n²) - When the array is already sorted and we choose the last element as pivot

### Space Complexity: O(log n) - Due to recursion stack

### Stability: No, quicksort is not stable

### Advantages:
- Fastest sorting algorithm in practice
- In-place sorting
- Cache-friendly

### Disadvantages:
- Worst case time complexity is O(n²)
- Not stable
- Performance depends on pivot selection
