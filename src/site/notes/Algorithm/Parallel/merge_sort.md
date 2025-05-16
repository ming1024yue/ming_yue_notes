---
{"dg-publish":true,"permalink":"/algorithm/parallel/merge-sort/"}
---

```python
import multiprocessing as mp
from concurrent.futures import ThreadPoolExecutor
import threading
import numpy as np

class ParallelMergeSort:
    def __init__(self, n_workers=None):
        """
        Parallel Merge Sort
        Time Complexity: O(n log n / p) where p is number of processors
        Space Complexity: O(n)
        """
        self.n_workers = n_workers or mp.cpu_count()
    
    def sort(self, arr):
        """Sort array using parallel merge sort"""
        if len(arr) <= 1:
            return arr
        
        # Split array into chunks
        chunk_size = len(arr) // self.n_workers
        chunks = [arr[i:i+chunk_size] for i in range(0, len(arr), chunk_size)]
        
        # Sort chunks in parallel
        with ThreadPoolExecutor(max_workers=self.n_workers) as executor:
            sorted_chunks = list(executor.map(self._sort_chunk, chunks))
        
        # Merge sorted chunks
        return self._merge_sorted_chunks(sorted_chunks)
    
    def _sort_chunk(self, chunk):
        """Sort a single chunk using sequential merge sort"""
        if len(chunk) <= 1:
            return chunk
        
        mid = len(chunk) // 2
        left = self._sort_chunk(chunk[:mid])
        right = self._sort_chunk(chunk[mid:])
        
        return self._merge(left, right)
    
    def _merge(self, left, right):
        """Merge two sorted arrays"""
        result = []
        i = j = 0
        
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1
        
        result.extend(left[i:])
        result.extend(right[j:])
        return result
    
    def _merge_sorted_chunks(self, chunks):
        """Merge multiple sorted chunks"""
        while len(chunks) > 1:
            new_chunks = []
            for i in range(0, len(chunks), 2):
                if i + 1 < len(chunks):
                    new_chunks.append(self._merge(chunks[i], chunks[i+1]))
                else:
                    new_chunks.append(chunks[i])
            chunks = new_chunks
        return chunks[0]

class ParallelMergeSortWithProcesses(ParallelMergeSort):
    def __init__(self, n_workers=None):
        super().__init__(n_workers)
        self.manager = mp.Manager()
    
    def sort(self, arr):
        """Sort array using parallel merge sort with processes"""
        if len(arr) <= 1:
            return arr
        
        # Create shared array
        shared_arr = self.manager.list(arr)
        
        # Create process pool
        with mp.Pool(processes=self.n_workers) as pool:
            # Sort chunks in parallel
            chunk_size = len(arr) // self.n_workers
            chunks = [(shared_arr, i, i+chunk_size) for i in range(0, len(arr), chunk_size)]
            sorted_chunks = pool.map(self._sort_chunk_process, chunks)
        
        # Merge sorted chunks
        return self._merge_sorted_chunks(sorted_chunks)
    
    def _sort_chunk_process(self, args):
        """Sort a chunk in a separate process"""
        arr, start, end = args
        chunk = list(arr[start:end])
        return self._sort_chunk(chunk)

class ParallelMergeSortWithBitonic(ParallelMergeSort):
    def sort(self, arr):
        """Sort array using parallel bitonic merge sort"""
        n = len(arr)
        if n <= 1:
            return arr
        
        # Pad array to power of 2
        next_power = 1 << (n-1).bit_length()
        padded = arr + [float('inf')] * (next_power - n)
        
        # Perform bitonic sort
        self._bitonic_sort(padded, 0, next_power, True)
        
        return padded[:n]
    
    def _bitonic_sort(self, arr, low, cnt, up):
        """Perform bitonic sort on array segment"""
        if cnt > 1:
            k = cnt // 2
            self._bitonic_sort(arr, low, k, True)
            self._bitonic_sort(arr, low + k, k, False)
            self._bitonic_merge(arr, low, cnt, up)
    
    def _bitonic_merge(self, arr, low, cnt, up):
        """Merge bitonic sequence"""
        if cnt > 1:
            k = cnt // 2
            for i in range(low, low + k):
                if (arr[i] > arr[i + k]) == up:
                    arr[i], arr[i + k] = arr[i + k], arr[i]
            self._bitonic_merge(arr, low, k, up)
            self._bitonic_merge(arr, low + k, k, up)

# Example usage
if __name__ == "__main__":
    # Create sample array
    arr = np.random.randint(0, 100, 16)
    print("Original array:", arr)
    
    # Basic parallel merge sort
    pms = ParallelMergeSort()
    sorted_arr1 = pms.sort(arr)
    print("\nBasic parallel merge sort result:", sorted_arr1)
    
    # Process-based parallel merge sort
    pms_process = ParallelMergeSortWithProcesses()
    sorted_arr2 = pms_process.sort(arr)
    print("\nProcess-based parallel merge sort result:", sorted_arr2)
    
    # Bitonic parallel merge sort
    pms_bitonic = ParallelMergeSortWithBitonic()
    sorted_arr3 = pms_bitonic.sort(arr)
    print("\nBitonic parallel merge sort result:", sorted_arr3)
    
    # Verify results
    print("\nVerification (should be True):")
    print(np.array_equal(sorted_arr1, sorted_arr2))
    print(np.array_equal(sorted_arr1, sorted_arr3))
```

## Explanation
Parallel Merge Sort implements different approaches to sort arrays using parallel processing.

### How it works:
1. Basic Parallel Merge Sort:
   - Divides array into chunks
   - Sorts chunks in parallel using threads
   - Merges sorted chunks sequentially

2. Process-based Parallel Merge Sort:
   - Uses processes instead of threads
   - Uses shared memory for data exchange
   - Better for CPU-bound tasks

3. Bitonic Parallel Merge Sort:
   - Uses bitonic sorting network
   - Naturally parallel algorithm
   - Requires array size to be power of 2

### Time Complexity:
- Basic: O(n log n / p)
- Process-based: O(n log n / p)
- Bitonic: O(log^2 n)

### Space Complexity: O(n)

### Applications:
- Large-scale data processing
- Database systems
- Scientific computing
- Machine learning
- Data analysis

### Advantages:
- Faster than sequential sort
- Scalable with number of processors
- Can handle large datasets
- Flexible implementation
- Good cache utilization

### Disadvantages:
- Overhead from parallelization
- Memory intensive
- Complex implementation
- May not be efficient for small arrays
- Requires careful tuning
