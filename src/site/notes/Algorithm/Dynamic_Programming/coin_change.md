---
{"dg-publish":true,"permalink":"/algorithm/dynamic-programming/coin-change/"}
---


```python
def coin_change(coins, amount):
    """
    Coin Change Problem using Dynamic Programming
    Time Complexity: O(nm)
    Space Complexity: O(m)
    where n is number of coins and m is amount
    """
    # Initialize DP array
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    # Fill DP array
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

def coin_change_ways(coins, amount):
    """
    Number of ways to make change using Dynamic Programming
    Time Complexity: O(nm)
    Space Complexity: O(m)
    """
    # Initialize DP array
    dp = [0] * (amount + 1)
    dp[0] = 1
    
    # Fill DP array
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] += dp[i - coin]
    
    return dp[amount]

def coin_change_with_solution(coins, amount):
    """
    Coin Change with solution reconstruction
    Time Complexity: O(nm)
    Space Complexity: O(m)
    """
    # Initialize DP arrays
    dp = [float('inf')] * (amount + 1)
    last_coin = [-1] * (amount + 1)
    dp[0] = 0
    
    # Fill DP arrays
    for coin in coins:
        for i in range(coin, amount + 1):
            if dp[i - coin] + 1 < dp[i]:
                dp[i] = dp[i - coin] + 1
                last_coin[i] = coin
    
    # Reconstruct solution
    if dp[amount] == float('inf'):
        return -1, []
    
    solution = []
    current = amount
    while current > 0:
        coin = last_coin[current]
        solution.append(coin)
        current -= coin
    
    return dp[amount], solution

# Example usage
if __name__ == "__main__":
    # Test case
    coins = [1, 2, 5]
    amount = 11
    
    # Get minimum number of coins
    min_coins = coin_change(coins, amount)
    print(f"Minimum number of coins: {min_coins}")
    
    # Get number of ways to make change
    num_ways = coin_change_ways(coins, amount)
    print(f"Number of ways to make change: {num_ways}")
    
    # Get solution with coins used
    min_coins, solution = coin_change_with_solution(coins, amount)
    print(f"Minimum coins: {min_coins}")
    print(f"Coins used: {solution}")
```

## Explanation
The Coin Change problem is to find the minimum number of coins needed to make a given amount of money, or to find the number of ways to make change for a given amount.

### How it works:
1. Initialize DP array with infinity (or 0 for ways)
2. Set base case (0 coins needed for amount 0)
3. Fill DP array in bottom-up manner:
   - For each coin
   - For each amount from coin to target
   - Update minimum coins or ways
4. Return result from DP array

### Time Complexity:
- Best Case: O(nm)
- Average Case: O(nm)
- Worst Case: O(nm)

### Space Complexity: O(m)

### Applications:
- Vending machines
- Cash register systems
- Financial calculations
- Resource allocation
- Budget planning

### Advantages:
- Optimal solution
- Works for any coin set
- Can find minimum coins
- Can count number of ways
- Can reconstruct solution

### Disadvantages:
- Pseudo-polynomial time
- Not suitable for large amounts
- Requires integer amounts
- May be memory intensive
- Limited to given coins 