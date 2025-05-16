---
{"dg-publish":true,"permalink":"/algorithm/greedy/offline-caching/"}
---

```python
def offline_caching(requests, cache_size):
    """
    Offline Caching using Farthest-in-Future Algorithm
    Time Complexity: O(n * k)
    Space Complexity: O(k)
    where n is the number of requests and k is the cache size
    """
    cache = set()
    cache_sequence = []
    cache_misses = 0
    
    for i, request in enumerate(requests):
        if request in cache:
            cache_sequence.append(list(cache))
            continue
        
        cache_misses += 1
        
        if len(cache) < cache_size:
            cache.add(request)
        else:
            # Find the farthest in future item to evict
            farthest = -1
            evict_item = None
            
            for item in cache:
                # Find next occurrence of this item
                next_occurrence = -1
                for j in range(i + 1, len(requests)):
                    if requests[j] == item:
                        next_occurrence = j
                        break
                
                if next_occurrence == -1:
                    evict_item = item
                    break
                
                if next_occurrence > farthest:
                    farthest = next_occurrence
                    evict_item = item
            
            cache.remove(evict_item)
            cache.add(request)
        
        cache_sequence.append(list(cache))
    
    return cache_sequence, cache_misses

def offline_caching_lru(requests, cache_size):
    """
    Offline Caching using Least Recently Used (LRU) Algorithm
    Time Complexity: O(n * k)
    Space Complexity: O(k)
    """
    cache = []
    cache_sequence = []
    cache_misses = 0
    
    for request in requests:
        if request in cache:
            # Move to end (most recently used)
            cache.remove(request)
            cache.append(request)
            cache_sequence.append(list(cache))
            continue
        
        cache_misses += 1
        
        if len(cache) < cache_size:
            cache.append(request)
        else:
            # Remove least recently used (first item)
            cache.pop(0)
            cache.append(request)
        
        cache_sequence.append(list(cache))
    
    return cache_sequence, cache_misses

def offline_caching_fifo(requests, cache_size):
    """
    Offline Caching using First-In-First-Out (FIFO) Algorithm
    Time Complexity: O(n * k)
    Space Complexity: O(k)
    """
    cache = []
    cache_sequence = []
    cache_misses = 0
    
    for request in requests:
        if request in cache:
            cache_sequence.append(list(cache))
            continue
        
        cache_misses += 1
        
        if len(cache) < cache_size:
            cache.append(request)
        else:
            # Remove first item
            cache.pop(0)
            cache.append(request)
        
        cache_sequence.append(list(cache))
    
    return cache_sequence, cache_misses

def offline_caching_lfu(requests, cache_size):
    """
    Offline Caching using Least Frequently Used (LFU) Algorithm
    Time Complexity: O(n * k)
    Space Complexity: O(k)
    """
    cache = {}
    cache_sequence = []
    cache_misses = 0
    frequencies = {}
    
    for request in requests:
        # Update frequency
        frequencies[request] = frequencies.get(request, 0) + 1
        
        if request in cache:
            cache[request] = frequencies[request]
            cache_sequence.append(list(cache.keys()))
            continue
        
        cache_misses += 1
        
        if len(cache) < cache_size:
            cache[request] = frequencies[request]
        else:
            # Find least frequently used item
            lfu_item = min(cache.items(), key=lambda x: x[1])[0]
            del cache[lfu_item]
            cache[request] = frequencies[request]
        
        cache_sequence.append(list(cache.keys()))
    
    return cache_sequence, cache_misses

# Example usage
if __name__ == "__main__":
    # Test case
    requests = ['A', 'B', 'C', 'A', 'D', 'E', 'F', 'A', 'B', 'C', 'D', 'E', 'F']
    cache_size = 3
    
    # Farthest-in-Future
    sequence, misses = offline_caching(requests, cache_size)
    print("Farthest-in-Future Algorithm:")
    print("Cache Sequence:")
    for i, cache in enumerate(sequence):
        print(f"Step {i+1}: {cache}")
    print(f"Cache Misses: {misses}")
    
    # LRU
    sequence, misses = offline_caching_lru(requests, cache_size)
    print("\nLRU Algorithm:")
    print("Cache Sequence:")
    for i, cache in enumerate(sequence):
        print(f"Step {i+1}: {cache}")
    print(f"Cache Misses: {misses}")
    
    # FIFO
    sequence, misses = offline_caching_fifo(requests, cache_size)
    print("\nFIFO Algorithm:")
    print("Cache Sequence:")
    for i, cache in enumerate(sequence):
        print(f"Step {i+1}: {cache}")
    print(f"Cache Misses: {misses}")
    
    # LFU
    sequence, misses = offline_caching_lfu(requests, cache_size)
    print("\nLFU Algorithm:")
    print("Cache Sequence:")
    for i, cache in enumerate(sequence):
        print(f"Step {i+1}: {cache}")
    print(f"Cache Misses: {misses}")
```

## Explanation
Offline Caching is a problem of managing a cache of fixed size to minimize cache misses when processing a sequence of requests.

### How it works:
1. Farthest-in-Future: Evict item whose next use is farthest in future
2. LRU: Evict least recently used item
3. FIFO: Evict first item that entered cache
4. LFU: Evict least frequently used item

### Time Complexity:
- Best Case: O(n * k)
- Average Case: O(n * k)
- Worst Case: O(n * k)

### Space Complexity: O(k)

### Applications:
- CPU cache management
- Web caching
- Database caching
- Memory management
- Resource allocation

### Advantages:
- Optimal solution (Farthest-in-Future)
- Simple implementation
- Easy to understand
- Works for many scenarios
- Efficient for small caches

### Disadvantages:
- Requires future knowledge
- May be slow for large caches
- Not suitable for all problems
- Memory intensive
- Requires careful analysis
