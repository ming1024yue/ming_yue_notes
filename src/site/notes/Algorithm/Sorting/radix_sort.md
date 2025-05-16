---
{"dg-publish":true,"permalink":"/algorithm/sorting/radix-sort/"}
---


# Radix Sort

```python
def counting_sort_for_radix(arr, exp):
    """
    A function to do counting sort of arr[] according to
    the digit represented by exp.
    """
    n = len(arr)
    output = [0] * n
    count = [0] * 10  # Since we are dealing with base 10 numbers

    # Store count of occurrences in count[]
    for i in range(n):
        index = (arr[i] // exp) % 10
        count[index] += 1

    # Change count[i] so that count[i] now contains actual
    # position of this digit in output[]
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Build the output array
    i = n - 1
    while i >= 0:
        index = (arr[i] // exp) % 10
        output[count[index] - 1] = arr[i]
        count[index] -= 1
        i -= 1

    # Copy the output array to arr[], so that arr[] now
    # contains sorted numbers according to current digit
    for i in range(n):
        arr[i] = output[i]

def radix_sort(arr):
    """
    Radix Sort Algorithm
    Time Complexity: O(d * (n + k)) where d is the number of digits
    Space Complexity: O(n + k)
    """
    # Find the maximum number to know number of digits
    max_val = max(arr)

    # Do counting sort for every digit. Note that instead
    # of passing digit number, exp is passed. exp is 10^i
    # where i is current digit number
    exp = 1
    while max_val // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10

# Example usage
if __name__ == "__main__":
    arr = [170, 45, 75, 90, 802, 24, 2, 66]
    print("Original array:", arr)
    radix_sort(arr)
    print("Sorted array:", arr)
```

## Explanation
Radix sort is a non-comparative sorting algorithm that sorts data with integer keys by grouping keys by the individual digits which share the same significant position and value. It uses counting sort as a subroutine to sort.

### How it works:
1. Find the maximum number to know the number of digits
2. Do counting sort for every digit from least significant digit to most significant digit
3. Use counting sort as a subroutine to sort the numbers based on the current digit

### Time Complexity:
- Best Case: O(d * (n + k))
- Average Case: O(d * (n + k))
- Worst Case: O(d * (n + k))

Where:
- d is the number of digits in the maximum number
- n is the number of elements in the array
- k is the range of the digit (10 for decimal numbers)

### Space Complexity: O(n + k)

### Stability: Yes, radix sort is stable

### Advantages:
- Fast when the keys are short
- Stable sorting algorithm
- Can be used for sorting strings of fixed length

### Disadvantages:
- Not suitable for floating point numbers
- Requires extra space
- Performance depends on the number of digits in the maximum number
- Not suitable for sorting large data sets with very large numbers
