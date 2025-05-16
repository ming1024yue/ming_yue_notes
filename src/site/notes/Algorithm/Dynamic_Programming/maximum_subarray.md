---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/maximum-subarray/"}
---


```python
def max_subarray(arr):
    """
    Maximum Subarray Problem using Dynamic Programming (Kadane's Algorithm)
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not arr:
        return 0, []
    
    max_sum = current_sum = arr[0]
    start = end = 0
    temp_start = 0
    
    for i in range(1, len(arr)):
        if arr[i] > current_sum + arr[i]:
            current_sum = arr[i]
            temp_start = i
        else:
            current_sum += arr[i]
        
        if current_sum > max_sum:
            max_sum = current_sum
            start = temp_start
            end = i
    
    return max_sum, arr[start:end+1]

def max_subarray_2d(matrix):
    """
    Maximum Subarray in 2D Matrix using Dynamic Programming
    Time Complexity: O(n³)
    Space Complexity: O(n²)
    """
    if not matrix or not matrix[0]:
        return 0, []
    
    rows, cols = len(matrix), len(matrix[0])
    max_sum = float('-inf')
    result = []
    
    # Try all possible left columns
    for left in range(cols):
        temp = [0] * rows
        
        # Try all possible right columns
        for right in range(left, cols):
            # Calculate sum between left and right for each row
            for i in range(rows):
                temp[i] += matrix[i][right]
            
            # Apply Kadane's algorithm on temp array
            current_sum, subarray = max_subarray(temp)
            
            if current_sum > max_sum:
                max_sum = current_sum
                result = subarray
    
    return max_sum, result

def max_subarray_circular(arr):
    """
    Maximum Subarray in Circular Array using Dynamic Programming
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not arr:
        return 0, []
    
    # Case 1: Maximum subarray is non-circular
    max_non_circular, _ = max_subarray(arr)
    
    # Case 2: Maximum subarray is circular
    total_sum = sum(arr)
    min_subarray, _ = max_subarray([-x for x in arr])
    max_circular = total_sum + min_subarray
    
    # Handle all negative numbers case
    if max_circular == 0:
        return max_non_circular, []
    
    return max(max_non_circular, max_circular), []

# Example usage
if __name__ == "__main__":
    # Test case for 1D array
    arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
    max_sum, subarray = max_subarray(arr)
    print(f"Maximum subarray sum: {max_sum}")
    print(f"Maximum subarray: {subarray}")
    
    # Test case for 2D array
    matrix = [
        [1, 2, -1, -4, -20],
        [-8, -3, 4, 2, 1],
        [3, 8, 10, 1, 3],
        [-4, -1, 1, 7, -6]
    ]
    max_sum_2d, subarray_2d = max_subarray_2d(matrix)
    print(f"\nMaximum 2D subarray sum: {max_sum_2d}")
    print(f"Maximum 2D subarray: {subarray_2d}")
    
    # Test case for circular array
    arr_circular = [5, -3, 5]
    max_sum_circular, _ = max_subarray_circular(arr_circular)
    print(f"\nMaximum circular subarray sum: {max_sum_circular}")
```

## Explanation
The Maximum Subarray problem is to find the contiguous subarray within a one-dimensional array of numbers that has the largest sum.

### How it works:
1. Initialize variables for tracking maximum and current sums
2. Iterate through array:
   - Update current sum
   - Update maximum sum if current is larger
   - Track start and end indices
3. Return maximum sum and subarray

### Time Complexity:
- 1D Array: O(n)
- 2D Array: O(n³)
- Circular Array: O(n)

### Space Complexity:
- 1D Array: O(1)
- 2D Array: O(n²)
- Circular Array: O(1)

### Applications:
- Stock market analysis
- Signal processing
- Image processing
- Data analysis
- Financial planning

### Advantages:
- Linear time for 1D
- Optimal solution
- Works for negative numbers
- Can handle circular arrays
- Can find subarray indices

### Disadvantages:
- Cubic time for 2D
- May be memory intensive
- Not suitable for very large n
- Limited to numerical data
- May be slow for some cases 