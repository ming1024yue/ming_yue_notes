---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/maximum-product-subarray/"}
---


# Maximum Product Subarray

```python
def max_product_subarray(nums):
    """
    Maximum Product Subarray using Dynamic Programming
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not nums:
        return 0
    
    n = len(nums)
    
    # Initialize variables
    max_product = min_product = result = nums[0]
    
    # Fill DP array
    for i in range(1, n):
        # Store current max and min
        current_max = max_product
        current_min = min_product
        
        # Update max and min
        max_product = max(nums[i], current_max * nums[i], current_min * nums[i])
        min_product = min(nums[i], current_max * nums[i], current_min * nums[i])
        
        # Update result
        result = max(result, max_product)
    
    return result

def max_product_subarray_with_subarray(nums):
    """
    Maximum Product Subarray with subarray reconstruction
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not nums:
        return 0, []
    
    n = len(nums)
    
    # Initialize variables
    max_product = min_product = result = nums[0]
    start = end = 0
    temp_start = 0
    
    # Fill DP array
    for i in range(1, n):
        # Store current max and min
        current_max = max_product
        current_min = min_product
        
        # Update max and min
        if nums[i] >= current_max * nums[i] and nums[i] >= current_min * nums[i]:
            max_product = nums[i]
            temp_start = i
        elif current_max * nums[i] >= current_min * nums[i]:
            max_product = current_max * nums[i]
        else:
            max_product = current_min * nums[i]
        
        if nums[i] <= current_max * nums[i] and nums[i] <= current_min * nums[i]:
            min_product = nums[i]
        elif current_max * nums[i] <= current_min * nums[i]:
            min_product = current_max * nums[i]
        else:
            min_product = current_min * nums[i]
        
        # Update result and indices
        if max_product > result:
            result = max_product
            start = temp_start
            end = i
    
    return result, nums[start:end+1]

def max_product_subarray_zeros(nums):
    """
    Maximum Product Subarray handling zeros
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    if not nums:
        return 0
    
    n = len(nums)
    
    # Initialize variables
    max_product = min_product = result = nums[0]
    
    # Fill DP array
    for i in range(1, n):
        if nums[i] == 0:
            # Reset max and min when encountering zero
            max_product = min_product = 0
        else:
            # Store current max and min
            current_max = max_product
            current_min = min_product
            
            # Update max and min
            max_product = max(nums[i], current_max * nums[i], current_min * nums[i])
            min_product = min(nums[i], current_max * nums[i], current_min * nums[i])
        
        # Update result
        result = max(result, max_product)
    
    return result

# Example usage
if __name__ == "__main__":
    # Test case
    nums = [2, 3, -2, 4]
    
    # Get maximum product
    max_product = max_product_subarray(nums)
    print(f"Maximum product: {max_product}")
    
    # Get maximum product with subarray
    max_product, subarray = max_product_subarray_with_subarray(nums)
    print(f"\nMaximum product: {max_product}")
    print(f"Subarray: {subarray}")
    
    # Test case with zeros
    nums_zeros = [2, 3, 0, 4, -2]
    max_product_zeros = max_product_subarray_zeros(nums_zeros)
    print(f"\nMaximum product (with zeros): {max_product_zeros}")
```

## Explanation
The Maximum Product Subarray problem is to find the contiguous subarray within an array that has the largest product.

### How it works:
1. Initialize variables for tracking maximum and minimum products
2. Iterate through array:
   - Store current max and min
   - Update max and min considering current number
   - Update result if current max is larger
3. Return maximum product

### Time Complexity:
- Best Case: O(n)
- Average Case: O(n)
- Worst Case: O(n)

### Space Complexity: O(1)

### Applications:
- Signal processing
- Image processing
- Financial analysis
- Data analysis
- Pattern recognition

### Advantages:
- Optimal solution
- Linear time complexity
- Constant space
- Can handle negative numbers
- Can reconstruct subarray

### Disadvantages:
- May be tricky to implement
- Requires careful handling of zeros
- Not suitable for very large n
- May be slow for some cases
- Limited to numerical data 