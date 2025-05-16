---
{"dg-publish":true,"permalink":"/algorithm/selection/minimum/"}
---


```python
def find_minimum(arr):
    """
    Find Minimum Element Algorithm
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not arr:
        return None
    
    min_element = arr[0]
    for i in range(1, len(arr)):
        if arr[i] < min_element:
            min_element = arr[i]
    return min_element

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6]
    print("Array:", arr)
    min_val = find_minimum(arr)
    print("Minimum element:", min_val)
```

## Explanation
Finding the minimum element in an array is a fundamental operation in computer science. The algorithm simply iterates through the array once, keeping track of the smallest element found so far.

### How it works:
1. Initialize min_element with the first element of the array
2. Iterate through the array from the second element
3. Compare each element with min_element
4. If current element is smaller, update min_element
5. Return min_element after the loop

### Time Complexity:
- Best Case: O(n)
- Average Case: O(n)
- Worst Case: O(n)

### Space Complexity: O(1)

### Applications:
- Sorting algorithms
- Priority queues
- Graph algorithms
- Data analysis
