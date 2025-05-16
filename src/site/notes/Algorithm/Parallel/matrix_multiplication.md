---
{"dg-publish":true,"permalink":"/algorithm/parallel/matrix-multiplication/"}
---

```python
import numpy as np
from multiprocessing import Pool, cpu_count
from concurrent.futures import ThreadPoolExecutor
import threading

class ParallelMatrixMultiplication:
    def __init__(self, n_workers=None):
        """
        Parallel Matrix Multiplication
        Time Complexity: O(n^3/p) where p is number of processors
        Space Complexity: O(n^2)
        """
        self.n_workers = n_workers or cpu_count()
    
    def multiply(self, A, B):
        """Multiply matrices A and B using parallel processing"""
        n = len(A)
        m = len(B[0])
        p = len(B)
        
        # Initialize result matrix
        C = np.zeros((n, m))
        
        # Create thread pool
        with ThreadPoolExecutor(max_workers=self.n_workers) as executor:
            # Submit tasks for each element in result matrix
            futures = []
            for i in range(n):
                for j in range(m):
                    futures.append(executor.submit(self._compute_element, A, B, i, j))
            
            # Collect results
            for future in futures:
                i, j, value = future.result()
                C[i][j] = value
        
        return C
    
    def _compute_element(self, A, B, i, j):
        """Compute single element of result matrix"""
        value = 0
        for k in range(len(B)):
            value += A[i][k] * B[k][j]
        return i, j, value

class ParallelMatrixMultiplicationWithBlocks(ParallelMatrixMultiplication):
    def __init__(self, n_workers=None, block_size=32):
        super().__init__(n_workers)
        self.block_size = block_size
    
    def multiply(self, A, B):
        """Multiply matrices using blocked parallel processing"""
        n = len(A)
        m = len(B[0])
        p = len(B)
        
        # Initialize result matrix
        C = np.zeros((n, m))
        
        # Create process pool
        with Pool(processes=self.n_workers) as pool:
            # Submit tasks for each block
            futures = []
            for i in range(0, n, self.block_size):
                for j in range(0, m, self.block_size):
                    futures.append(pool.apply_async(
                        self._compute_block,
                        (A, B, i, j, min(self.block_size, n-i), min(self.block_size, m-j))
                    ))
            
            # Collect results
            for future in futures:
                block_i, block_j, block = future.get()
                C[block_i:block_i+len(block), block_j:block_j+len(block[0])] = block
        
        return C
    
    def _compute_block(self, A, B, start_i, start_j, block_size_i, block_size_j):
        """Compute a block of the result matrix"""
        block = np.zeros((block_size_i, block_size_j))
        for i in range(block_size_i):
            for j in range(block_size_j):
                for k in range(len(B)):
                    block[i][j] += A[start_i + i][k] * B[k][start_j + j]
        return start_i, start_j, block

class ParallelMatrixMultiplicationWithStrassen(ParallelMatrixMultiplication):
    def multiply(self, A, B):
        """Multiply matrices using Strassen's algorithm with parallel processing"""
        n = len(A)
        
        # Base case
        if n <= 32:
            return super().multiply(A, B)
        
        # Split matrices into quadrants
        mid = n // 2
        A11, A12, A21, A22 = self._split_matrix(A, mid)
        B11, B12, B21, B22 = self._split_matrix(B, mid)
        
        # Create thread pool
        with ThreadPoolExecutor(max_workers=self.n_workers) as executor:
            # Compute Strassen's algorithm steps in parallel
            P1 = executor.submit(self.multiply, A11, self._subtract(B12, B22))
            P2 = executor.submit(self.multiply, self._add(A11, A12), B22)
            P3 = executor.submit(self.multiply, self._add(A21, A22), B11)
            P4 = executor.submit(self.multiply, A22, self._subtract(B21, B11))
            P5 = executor.submit(self.multiply, self._add(A11, A22), self._add(B11, B22))
            P6 = executor.submit(self.multiply, self._subtract(A12, A22), self._add(B21, B22))
            P7 = executor.submit(self.multiply, self._subtract(A11, A21), self._add(B11, B12))
            
            # Get results
            P1, P2, P3, P4, P5, P6, P7 = [f.result() for f in [P1, P2, P3, P4, P5, P6, P7]]
        
        # Combine results
        C11 = self._add(self._subtract(self._add(P5, P4), P2), P6)
        C12 = self._add(P1, P2)
        C21 = self._add(P3, P4)
        C22 = self._subtract(self._subtract(self._add(P5, P1), P3), P7)
        
        # Combine quadrants
        return self._combine_matrices(C11, C12, C21, C22)
    
    def _split_matrix(self, matrix, mid):
        """Split matrix into quadrants"""
        return (
            matrix[:mid, :mid],
            matrix[:mid, mid:],
            matrix[mid:, :mid],
            matrix[mid:, mid:]
        )
    
    def _combine_matrices(self, C11, C12, C21, C22):
        """Combine quadrants into single matrix"""
        n = len(C11) * 2
        C = np.zeros((n, n))
        mid = n // 2
        C[:mid, :mid] = C11
        C[:mid, mid:] = C12
        C[mid:, :mid] = C21
        C[mid:, mid:] = C22
        return C
    
    def _add(self, A, B):
        """Add two matrices"""
        return A + B
    
    def _subtract(self, A, B):
        """Subtract two matrices"""
        return A - B

# Example usage
if __name__ == "__main__":
    # Create sample matrices
    A = np.random.rand(4, 4)
    B = np.random.rand(4, 4)
    
    # Basic parallel multiplication
    pmm = ParallelMatrixMultiplication()
    C1 = pmm.multiply(A, B)
    print("Basic parallel multiplication result:")
    print(C1)
    
    # Blocked parallel multiplication
    pmm_block = ParallelMatrixMultiplicationWithBlocks(block_size=2)
    C2 = pmm_block.multiply(A, B)
    print("\nBlocked parallel multiplication result:")
    print(C2)
    
    # Strassen's parallel multiplication
    pmm_strassen = ParallelMatrixMultiplicationWithStrassen()
    C3 = pmm_strassen.multiply(A, B)
    print("\nStrassen's parallel multiplication result:")
    print(C3)
    
    # Verify results
    print("\nVerification (should be close to zero):")
    print(np.max(np.abs(C1 - C2)))
    print(np.max(np.abs(C1 - C3)))
```

## Explanation
Parallel Matrix Multiplication implements different approaches to multiply matrices using parallel processing.

### How it works:
1. Basic Parallel Multiplication:
   - Uses thread pool to compute each element in parallel
   - Each thread computes one element of result matrix

2. Blocked Parallel Multiplication:
   - Divides matrices into blocks
   - Uses process pool to compute blocks in parallel
   - Reduces memory access overhead

3. Strassen's Parallel Multiplication:
   - Uses Strassen's algorithm with parallel processing
   - Recursively divides matrices into quadrants
   - Computes intermediate products in parallel

### Time Complexity:
- Basic: O(n^3/p)
- Blocked: O(n^3/p)
- Strassen: O(n^log2(7)/p)

### Space Complexity: O(n^2)

### Applications:
- Scientific computing
- Machine learning
- Computer graphics
- Signal processing
- Image processing

### Advantages:
- Faster than sequential multiplication
- Better cache utilization
- Scalable with number of processors
- Can handle large matrices
- Flexible implementation

### Disadvantages:
- Overhead from parallelization
- Memory intensive
- Complex implementation
- May not be efficient for small matrices
- Requires careful tuning
