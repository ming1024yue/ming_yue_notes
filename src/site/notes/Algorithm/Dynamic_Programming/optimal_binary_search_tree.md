---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/optimal-binary-search-tree/"}
---

```python
def optimal_bst(keys, freq):
    """
    Optimal Binary Search Tree using Dynamic Programming
    Time Complexity: O(n³)
    Space Complexity: O(n²)
    """
    n = len(keys)
    
    # Create cost and root tables
    cost = [[0] * n for _ in range(n)]
    root = [[0] * n for _ in range(n)]
    
    # Initialize cost for single nodes
    for i in range(n):
        cost[i][i] = freq[i]
        root[i][i] = i
    
    # Fill tables in bottom-up manner
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            cost[i][j] = float('inf')
            
            # Get sum of frequencies from i to j
            freq_sum = sum(freq[i:j + 1])
            
            # Try all possible roots
            for r in range(i, j + 1):
                c = 0
                if r > i:
                    c += cost[i][r - 1]
                if r < j:
                    c += cost[r + 1][j]
                c += freq_sum
                
                if c < cost[i][j]:
                    cost[i][j] = c
                    root[i][j] = r
    
    return cost[0][n - 1], root

def construct_optimal_bst(keys, root, i, j):
    """
    Construct optimal BST from root table
    """
    if i > j:
        return None
    
    r = root[i][j]
    node = {
        'key': keys[r],
        'left': construct_optimal_bst(keys, root, i, r - 1),
        'right': construct_optimal_bst(keys, root, r + 1, j)
    }
    
    return node

def print_bst(node, level=0):
    """
    Print BST structure
    """
    if node is None:
        return
    
    print_bst(node['right'], level + 1)
    print(" " * 4 * level + str(node['key']))
    print_bst(node['left'], level + 1)

# Example usage
if __name__ == "__main__":
    # Test case
    keys = [10, 12, 20]
    freq = [34, 8, 50]
    
    # Get minimum cost and root table
    min_cost, root_table = optimal_bst(keys, freq)
    print(f"Minimum cost of BST: {min_cost}")
    
    # Construct and print optimal BST
    print("\nOptimal BST structure:")
    bst = construct_optimal_bst(keys, root_table, 0, len(keys) - 1)
    print_bst(bst)
```

## Explanation
The Optimal Binary Search Tree problem is to construct a binary search tree that minimizes the expected search cost, given the frequency of searches for each key.

### How it works:
1. Create cost and root tables
2. Initialize cost for single nodes
3. Fill tables in bottom-up manner:
   - For each possible length of subtree
   - For each starting position
   - Try all possible roots
   - Calculate cost and update tables
4. Construct optimal BST from root table

### Time Complexity:
- Best Case: O(n³)
- Average Case: O(n³)
- Worst Case: O(n³)

### Space Complexity: O(n²)

### Applications:
- Database indexing
- Dictionary implementation
- Symbol tables
- Compiler design
- Information retrieval

### Advantages:
- Optimal solution
- Efficient for small to medium n
- Easy to implement
- Can be extended to other problems
- Works for any frequency distribution

### Disadvantages:
- Cubic time complexity
- Not suitable for very large n
- Requires frequency table
- Limited to binary search trees
- May not be optimal for all cases 