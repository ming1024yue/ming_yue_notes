---
{"dg-publish":true,"permalink":"/algorithm/greedy/huffman-coding/"}
---

```python
import heapq
from collections import defaultdict

class HuffmanNode:
    def __init__(self, char=None, freq=0, left=None, right=None):
        self.char = char
        self.freq = freq
        self.left = left
        self.right = right
    
    def __lt__(self, other):
        return self.freq < other.freq

def build_huffman_tree(freq_dict):
    """
    Build Huffman Tree
    Time Complexity: O(n log n)
    Space Complexity: O(n)
    where n is the number of unique characters
    """
    # Create priority queue
    heap = []
    for char, freq in freq_dict.items():
        heapq.heappush(heap, HuffmanNode(char, freq))
    
    # Build tree
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged = HuffmanNode(freq=left.freq + right.freq, left=left, right=right)
        heapq.heappush(heap, merged)
    
    return heap[0]

def build_codes(root, current_code="", codes=None):
    """
    Build Huffman Codes
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    if codes is None:
        codes = {}
    
    if root.char is not None:
        codes[root.char] = current_code
        return
    
    build_codes(root.left, current_code + "0", codes)
    build_codes(root.right, current_code + "1", codes)
    
    return codes

def huffman_encode(text):
    """
    Huffman Encoding
    Time Complexity: O(n log n)
    Space Complexity: O(n)
    """
    # Calculate frequencies
    freq_dict = defaultdict(int)
    for char in text:
        freq_dict[char] += 1
    
    # Build tree and codes
    root = build_huffman_tree(freq_dict)
    codes = build_codes(root)
    
    # Encode text
    encoded = ""
    for char in text:
        encoded += codes[char]
    
    return encoded, codes

def huffman_decode(encoded, codes):
    """
    Huffman Decoding
    Time Complexity: O(n)
    Space Complexity: O(n)
    """
    # Reverse code dictionary
    reverse_codes = {code: char for char, code in codes.items()}
    
    # Decode text
    current_code = ""
    decoded = ""
    for bit in encoded:
        current_code += bit
        if current_code in reverse_codes:
            decoded += reverse_codes[current_code]
            current_code = ""
    
    return decoded

def huffman_compression_ratio(original, encoded):
    """
    Calculate compression ratio
    Time Complexity: O(1)
    Space Complexity: O(1)
    """
    original_bits = len(original) * 8  # Assuming 8 bits per character
    encoded_bits = len(encoded)
    return original_bits / encoded_bits

# Example usage
if __name__ == "__main__":
    # Test case
    text = "this is an example of huffman encoding"
    
    # Encode
    encoded, codes = huffman_encode(text)
    print("Original Text:", text)
    print("Encoded Text:", encoded)
    print("\nHuffman Codes:")
    for char, code in sorted(codes.items()):
        print(f"'{char}': {code}")
    
    # Decode
    decoded = huffman_decode(encoded, codes)
    print("\nDecoded Text:", decoded)
    
    # Compression ratio
    ratio = huffman_compression_ratio(text, encoded)
    print(f"\nCompression Ratio: {ratio:.2f}:1")
```

## Explanation
Huffman Coding is a lossless data compression algorithm that uses variable-length codes to represent characters.

### How it works:
1. Calculate frequency of each character
2. Build Huffman tree using priority queue
3. Generate codes by traversing tree
4. Encode text using generated codes
5. Decode using same tree structure

### Time Complexity:
- Best Case: O(n log n)
- Average Case: O(n log n)
- Worst Case: O(n log n)

### Space Complexity: O(n)

### Applications:
- Data compression
- File compression
- Network transmission
- Image compression
- Text compression

### Advantages:
- Optimal prefix code
- Lossless compression
- Efficient for text
- Simple implementation
- Widely used

### Disadvantages:
- Two-pass algorithm
- Not suitable for all data
- Requires frequency table
- May be slow for large data
- Memory intensive 