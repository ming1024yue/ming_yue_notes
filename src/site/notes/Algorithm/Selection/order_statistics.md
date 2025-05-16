---
{"dg-publish":true,"permalink":"/algorithm/selection/order-statistics/"}
---

```python
def partition(arr, low, high):
    """
    Helper function for quickselect
    """
    pivot = arr[high]
    i = low - 1
    
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quickselect(arr, low, high, k):
    """
    Quickselect Algorithm for finding kth smallest element
    Time Complexity: O(n) average case, O(n²) worst case
    Space Complexity: O(1)
    """
    if low == high:
        return arr[low]
    
    pivot_index = partition(arr, low, high)
    
    if k == pivot_index:
        return arr[k]
    elif k < pivot_index:
        return quickselect(arr, low, pivot_index - 1, k)
    else:
        return quickselect(arr, pivot_index + 1, high, k)

def find_kth_smallest(arr, k):
    """
    Find kth smallest element in array
    """
    if not arr or k < 1 or k > len(arr):
        return None
    
    # k-1 because array is 0-indexed
    return quickselect(arr, 0, len(arr) - 1, k - 1)

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6]
    k = 3
    print("Array:", arr)
    kth_smallest = find_kth_smallest(arr, k)
    print(f"{k}th smallest element:", kth_smallest)
```

## Explanation
Order statistics is the problem of finding the kth smallest element in an unsorted array. The Quickselect algorithm is an efficient solution that uses the same partitioning approach as Quicksort.

### How it works:
1. Choose a pivot element and partition the array
2. If the pivot is the kth element, return it
3. If k is less than the pivot index, search in the left subarray
4. If k is greater than the pivot index, search in the right subarray

### Time Complexity:
- Best Case: O(n)
- Average Case: O(n)
- Worst Case: O(n²) - When the array is already sorted and we choose the last element as pivot

### Space Complexity: O(1) - In-place algorithm

### Applications:
- Finding median
- Finding quartiles
- Finding percentiles
- Finding minimum/maximum
- Finding any order statistic

### Advantages:
- Average case linear time complexity
- In-place algorithm
- No extra space required

### Disadvantages:
- Worst case quadratic time complexity
- Not stable
- Performance depends on pivot selection
