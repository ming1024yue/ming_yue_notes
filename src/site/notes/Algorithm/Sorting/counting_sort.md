---
{"dg-publish":true,"permalink":"/algorithm/sorting/counting-sort/"}
---


# Counting Sort

```python
def counting_sort(arr):
    """
    Counting Sort Algorithm
    Time Complexity: O(n + k) where k is the range of input
    Space Complexity: O(n + k)
    """
    # The output array that will have sorted arr
    output = [0] * len(arr)
    
    # Create a count array to store count of individual elements
    # and initialize count array as 0
    max_val = max(arr)
    count = [0] * (max_val + 1)
    
    # Store count of each element
    for i in range(len(arr)):
        count[arr[i]] += 1
    
    # Change count[i] so that count[i] now contains actual
    # position of this element in output array
    for i in range(1, max_val + 1):
        count[i] += count[i - 1]
    
    # Build the output array
    i = len(arr) - 1
    while i >= 0:
        output[count[arr[i]] - 1] = arr[i]
        count[arr[i]] -= 1
        i -= 1
    
    # Copy the output array to arr, so that arr now
    # contains sorted elements
    for i in range(len(arr)):
        arr[i] = output[i]

# Example usage
if __name__ == "__main__":
    arr = [4, 2, 2, 8, 3, 3, 1]
    print("Original array:", arr)
    counting_sort(arr)
    print("Sorted array:", arr)
```

## Explanation
Counting sort is a sorting algorithm that works by counting the number of objects having distinct key values. It is a non-comparison based sorting algorithm that works well when the range of input data is not significantly greater than the number of objects to be sorted.

### How it works:
1. Find the maximum element in the array
2. Create a count array of size max+1 and initialize it with zeros
3. Store the count of each element at their respective index in count array
4. Store cumulative sum of the elements of the count array
5. Find the index of each element of the original array in count array
6. Place the elements in output array

### Time Complexity:
- Best Case: O(n + k)
- Average Case: O(n + k)
- Worst Case: O(n + k)

### Space Complexity: O(n + k)

### Stability: Yes, counting sort is stable

### Advantages:
- Linear time complexity
- Stable sorting algorithm
- Works well when the range of input data is not significantly greater than the number of objects to be sorted

### Disadvantages:
- Not suitable for sorting large data sets
- Not suitable for sorting floating point numbers
- Requires extra space for the count array
- Requires prior knowledge of the range of input data
