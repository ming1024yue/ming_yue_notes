---
{"dg-publish":true,"permalink":"/algorithm/selection/median/"}
---

```python
def find_median(arr):
    """
    Find Median Element Algorithm
    Time Complexity: O(n log n) due to sorting
    Space Complexity: O(n)
    """
    if not arr:
        return None
    
    # Create a copy of the array to avoid modifying the original
    sorted_arr = sorted(arr)
    n = len(sorted_arr)
    
    # If length is odd, return middle element
    if n % 2 != 0:
        return sorted_arr[n // 2]
    # If length is even, return average of two middle elements
    else:
        return (sorted_arr[n // 2 - 1] + sorted_arr[n // 2]) / 2

# Example usage
if __name__ == "__main__":
    arr = [12, 11, 13, 5, 6]
    print("Array:", arr)
    median_val = find_median(arr)
    print("Median:", median_val)
```

## Explanation
The median is the middle value in a sorted list of numbers. For an odd number of elements, it's the middle element. For an even number of elements, it's the average of the two middle elements.

### How it works:
1. Sort the array (using built-in sort for simplicity)
2. If the array length is odd:
   - Return the middle element
3. If the array length is even:
   - Return the average of the two middle elements

### Time Complexity:
- Best Case: O(n log n) - Due to sorting
- Average Case: O(n log n)
- Worst Case: O(n log n)

### Space Complexity: O(n) - Due to creating a sorted copy

### Applications:
- Statistics and data analysis
- Image processing
- Machine learning
- Data compression
- Signal processing

### Note:
This implementation uses sorting for simplicity. There are more efficient algorithms (like Quickselect) that can find the median in O(n) average time, but they are more complex to implement.
