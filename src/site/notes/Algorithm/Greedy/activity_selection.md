---
{"dg-publish":true,"permalink":"/algorithm/greedy/activity-selection/"}
---

```python
def activity_selection(activities):
    """
    Activity Selection Problem using Greedy Algorithm
    Time Complexity: O(n log n)
    Space Complexity: O(n)
    where n is the number of activities
    """
    # Sort activities by finish time
    sorted_activities = sorted(activities, key=lambda x: x[1])
    
    # Initialize result with first activity
    result = [sorted_activities[0]]
    last_finish = sorted_activities[0][1]
    
    # Select activities
    for activity in sorted_activities[1:]:
        start, finish = activity
        if start >= last_finish:
            result.append(activity)
            last_finish = finish
    
    return result

def activity_selection_with_weights(activities):
    """
    Activity Selection with Weights using Dynamic Programming
    Time Complexity: O(n²)
    Space Complexity: O(n)
    """
    # Sort activities by finish time
    sorted_activities = sorted(activities, key=lambda x: x[1])
    n = len(sorted_activities)
    
    # Initialize DP array
    dp = [0] * n
    dp[0] = sorted_activities[0][2]  # weight of first activity
    
    # Fill DP array
    for i in range(1, n):
        # Find last compatible activity
        j = i - 1
        while j >= 0 and sorted_activities[j][1] > sorted_activities[i][0]:
            j -= 1
        
        # Include current activity
        include = sorted_activities[i][2] + (dp[j] if j >= 0 else 0)
        # Exclude current activity
        exclude = dp[i-1]
        
        dp[i] = max(include, exclude)
    
    return dp[-1]

def activity_selection_with_deadlines(activities):
    """
    Activity Selection with Deadlines using Greedy Algorithm
    Time Complexity: O(n log n)
    Space Complexity: O(n)
    """
    # Sort activities by deadline
    sorted_activities = sorted(activities, key=lambda x: x[2])
    
    # Initialize result and time
    result = []
    current_time = 0
    
    # Select activities
    for activity in sorted_activities:
        start, duration, deadline = activity
        if current_time + duration <= deadline:
            result.append(activity)
            current_time += duration
    
    return result

# Example usage
if __name__ == "__main__":
    # Test case 1: Basic activity selection
    activities = [
        (1, 4), (3, 5), (0, 6), (5, 7),
        (3, 8), (5, 9), (6, 10), (8, 11),
        (8, 12), (2, 13), (12, 14)
    ]
    
    selected = activity_selection(activities)
    print("Selected Activities:")
    for start, finish in selected:
        print(f"Start: {start}, Finish: {finish}")
    
    # Test case 2: Activity selection with weights
    weighted_activities = [
        (1, 4, 2), (3, 5, 4), (0, 6, 3), (5, 7, 1),
        (3, 8, 5), (5, 9, 2), (6, 10, 4), (8, 11, 3)
    ]
    
    max_weight = activity_selection_with_weights(weighted_activities)
    print(f"\nMaximum Weight: {max_weight}")
    
    # Test case 3: Activity selection with deadlines
    deadline_activities = [
        (1, 2, 4), (3, 1, 5), (0, 3, 6), (5, 2, 7),
        (3, 2, 8), (5, 1, 9), (6, 2, 10), (8, 1, 11)
    ]
    
    selected_deadline = activity_selection_with_deadlines(deadline_activities)
    print("\nSelected Activities with Deadlines:")
    for start, duration, deadline in selected_deadline:
        print(f"Start: {start}, Duration: {duration}, Deadline: {deadline}")
```

## Explanation
The Activity Selection problem is a classic greedy algorithm problem that involves selecting the maximum number of non-overlapping activities.

### How it works:
1. Sort activities by finish time
2. Select first activity
3. For each remaining activity:
   - If it doesn't overlap with last selected activity, select it
   - Otherwise, skip it

### Time Complexity:
- Best Case: O(n log n)
- Average Case: O(n log n)
- Worst Case: O(n log n)

### Space Complexity: O(n)

### Applications:
- Scheduling problems
- Resource allocation
- Time management
- Project planning
- Task scheduling

### Advantages:
- Optimal solution
- Simple implementation
- Efficient for large inputs
- Works for many variations
- Easy to understand

### Disadvantages:
- Requires sorting
- Not suitable for all problems
- May need modifications
- Limited to specific cases
- Requires careful analysis
