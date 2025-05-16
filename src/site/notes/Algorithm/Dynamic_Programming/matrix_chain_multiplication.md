---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/matrix-chain-multiplication/"}
---

```python
def matrix_chain_multiplication(dimensions):
    """
    Matrix Chain Multiplication using Dynamic Programming
    Time Complexity: O(n³)
    Space Complexity: O(n²)
    """
    n = len(dimensions) - 1
    # Create tables for minimum costs and split points
    m = [[0] * n for _ in range(n)]
    s = [[0] * n for _ in range(n)]
    
    # Fill tables in bottom-up manner
    for length in range(2, n + 1):  # length of chain
        for i in range(n - length + 1):
            j = i + length - 1
            m[i][j] = float('inf')
            for k in range(i, j):
                cost = m[i][k] + m[k + 1][j] + dimensions[i] * dimensions[k + 1] * dimensions[j + 1]
                if cost < m[i][j]:
                    m[i][j] = cost
                    s[i][j] = k
    
    return m[0][n - 1], s

def print_optimal_parenthesization(s, i, j):
    """
    Print optimal parenthesization of matrix chain
    """
    if i == j:
        print(f"A{i + 1}", end="")
    else:
        print("(", end="")
        print_optimal_parenthesization(s, i, s[i][j])
        print_optimal_parenthesization(s, s[i][j] + 1, j)
        print(")", end="")

# Example usage
if __name__ == "__main__":
    # Matrix dimensions: A1(30x35), A2(35x15), A3(15x5), A4(5x10), A5(10x20), A6(20x25)
    dimensions = [30, 35, 15, 5, 10, 20, 25]
    
    # Get minimum cost and split points
    min_cost, split_points = matrix_chain_multiplication(dimensions)
    print(f"Minimum number of multiplications: {min_cost}")
    
    # Print optimal parenthesization
    print("Optimal parenthesization: ", end="")
    print_optimal_parenthesization(split_points, 0, len(dimensions) - 2)
    print()
```

## Explanation
The Matrix Chain Multiplication problem is to determine the optimal way to parenthesize a chain of matrices to minimize the number of scalar multiplications.

### How it works:
1. Create tables for minimum costs and split points
2. Fill tables in bottom-up manner:
   - For each chain length from 2 to n
   - For each starting position i
   - For each possible split point k
   - Calculate cost and update tables
3. Return minimum cost and split points

### Time Complexity:
- Best Case: O(n³)
- Average Case: O(n³)
- Worst Case: O(n³)

### Space Complexity: O(n²)

### Applications:
- Matrix operations
- Computer graphics
- Scientific computing
- Machine learning
- Optimization problems

### Advantages:
- Optimal solution
- Efficient for small to medium n
- Easy to implement
- Can be extended to other problems
- Works for any matrix dimensions

### Disadvantages:
- Cubic time complexity
- Not suitable for very large n
- Requires dimension table
- Limited to matrix multiplication
- May not be optimal for all cases
