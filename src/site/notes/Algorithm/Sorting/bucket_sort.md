---
{"dg-publish":true,"permalink":"/algorithm/sorting/bucket-sort/"}
---


# Bucket Sort

```python
def bucket_sort(arr):
    """
    Bucket Sort Algorithm
    Time Complexity: O(n²) in worst case, O(n) in best case
    Space Complexity: O(n + k) where k is the number of buckets
    """
    # Find maximum value in the array and use length of array to determine which value in the array goes into which bucket
    max_value = max(arr)
    size = max_value / len(arr)

    # Create n empty buckets where n is equal to the length of the input array
    buckets = [[] for _ in range(len(arr))]

    # Put array elements in different buckets
    for i in range(len(arr)):
        j = int(arr[i] / size)
        if j != len(arr):
            buckets[j].append(arr[i])
        else:
            buckets[len(arr) - 1].append(arr[i])

    # Sort individual buckets
    for i in range(len(arr)):
        buckets[i].sort()

    # Concatenate all buckets into arr[]
    index = 0
    for i in range(len(arr)):
        for j in range(len(buckets[i])):
            arr[index] = buckets[i][j]
            index += 1

# Example usage
if __name__ == "__main__":
    arr = [0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434]
    print("Original array:", arr)
    bucket_sort(arr)
    print("Sorted array:", arr)
```

## Explanation
Bucket sort is a sorting algorithm that works by distributing the elements of an array into a number of buckets. Each bucket is then sorted individually, either using a different sorting algorithm, or by recursively applying the bucket sorting algorithm.

### How it works:
1. Create n empty buckets (or lists)
2. Insert array elements into different buckets based on their values
3. Sort individual buckets using insertion sort
4. Concatenate all sorted buckets

### Time Complexity:
- Best Case: O(n) - When the input is uniformly distributed
- Average Case: O(n + n²/k) where k is the number of buckets
- Worst Case: O(n²) - When all elements are placed in a single bucket

### Space Complexity: O(n + k) where k is the number of buckets

### Stability: Yes, bucket sort is stable

### Advantages:
- Fast when the input is uniformly distributed
- Can be used for sorting floating point numbers
- Can be used for sorting strings

### Disadvantages:
- Not suitable for sorting large data sets
- Performance depends on the distribution of the input data
- Requires extra space for the buckets
- Not suitable for sorting data with a large range of values
