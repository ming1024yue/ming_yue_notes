---
{"dg-publish":true,"permalink":"/algorithm/selection/maximum/"}
---

```python
def find_maximum(arr):
    """
    Find Maximum Element Algorithm
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not arr:
        return None
    
    max_element = arr[0]
    for i in range(1, len(arr)):
        if arr[i] > max_element:
            max_element = arr[i]
    return max_element

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6]
    print("Array:", arr)
    max_val = find_maximum(arr)
    print("Maximum element:", max_val)
```

## Explanation
Finding the maximum element in an array is a fundamental operation in computer science. The algorithm simply iterates through the array once, keeping track of the largest element found so far.

### How it works:
1. Initialize max_element with the first element of the array
2. Iterate through the array from the second element
3. Compare each element with max_element
4. If current element is larger, update max_element
5. Return max_element after the loop

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
- Statistics and data science
