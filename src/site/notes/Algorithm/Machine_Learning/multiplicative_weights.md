---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/multiplicative-weights/"}
---

```python
import numpy as np

class MultiplicativeWeights:
    def __init__(self, n_experts, learning_rate=0.5):
        """
        Multiplicative-weights Algorithm
        Time Complexity: O(n * T)
        Space Complexity: O(n)
        where n is number of experts and T is number of rounds
        """
        self.n_experts = n_experts
        self.learning_rate = learning_rate
        self.weights = np.ones(n_experts) / n_experts
        self.cumulative_loss = np.zeros(n_experts)
    
    def predict(self):
        return np.random.choice(self.n_experts, p=self.weights)
    
    def update(self, losses):
        # Update cumulative loss
        self.cumulative_loss += losses
        
        # Update weights
        self.weights *= np.exp(-self.learning_rate * losses)
        self.weights /= np.sum(self.weights)
    
    def get_regret(self):
        return np.min(self.cumulative_loss) - np.sum(self.weights * self.cumulative_loss)

class Hedge(MultiplicativeWeights):
    def __init__(self, n_experts, learning_rate=0.5):
        super().__init__(n_experts, learning_rate)
    
    def predict(self):
        return np.argmax(self.weights)

class AdaHedge(MultiplicativeWeights):
    def __init__(self, n_experts):
        super().__init__(n_experts)
        self.cumulative_weights = np.zeros(n_experts)
        self.cumulative_losses = np.zeros(n_experts)
        self.learning_rate = 1.0
    
    def update(self, losses):
        # Update cumulative weights and losses
        self.cumulative_weights += self.weights
        self.cumulative_losses += losses
        
        # Update learning rate
        self.learning_rate = np.sqrt(np.log(self.n_experts) / (self.cumulative_weights * losses**2).sum())
        
        # Update weights
        self.weights *= np.exp(-self.learning_rate * losses)
        self.weights /= np.sum(self.weights)

class NormalHedge(MultiplicativeWeights):
    def __init__(self, n_experts):
        super().__init__(n_experts)
        self.cumulative_losses = np.zeros(n_experts)
    
    def update(self, losses):
        # Update cumulative losses
        self.cumulative_losses += losses
        
        # Compute potential function
        def potential(x):
            return np.exp(x**2 / 2)
        
        # Update weights
        self.weights = potential(self.cumulative_losses - np.min(self.cumulative_losses))
        self.weights /= np.sum(self.weights)

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    n_experts = 5
    n_rounds = 100
    
    # Generate random losses
    losses = np.random.rand(n_rounds, n_experts)
    
    # Basic Multiplicative-weights
    mw = MultiplicativeWeights(n_experts)
    for t in range(n_rounds):
        mw.update(losses[t])
    print("Basic Multiplicative-weights Regret:", mw.get_regret())
    
    # Hedge
    hedge = Hedge(n_experts)
    for t in range(n_rounds):
        hedge.update(losses[t])
    print("Hedge Regret:", hedge.get_regret())
    
    # AdaHedge
    ada_hedge = AdaHedge(n_experts)
    for t in range(n_rounds):
        ada_hedge.update(losses[t])
    print("AdaHedge Regret:", ada_hedge.get_regret())
    
    # NormalHedge
    normal_hedge = NormalHedge(n_experts)
    for t in range(n_rounds):
        normal_hedge.update(losses[t])
    print("NormalHedge Regret:", normal_hedge.get_regret())
```

## Explanation
Multiplicative-weights algorithms are a family of online learning algorithms that maintain a distribution over experts and update it based on their performance.

### How it works:
1. Initialize uniform weights over experts
2. For each round:
   - Make prediction based on current weights
   - Observe losses
   - Update weights multiplicatively
3. Track cumulative regret

### Time Complexity:
- Best Case: O(n * T)
- Average Case: O(n * T)
- Worst Case: O(n * T)

### Space Complexity: O(n)

### Applications:
- Online learning
- Portfolio management
- Game theory
- Boosting algorithms
- Expert advice

### Advantages:
- Simple implementation
- Strong theoretical guarantees
- Adapts to best expert
- Works in adversarial settings
- Low regret bounds

### Disadvantages:
- Sensitive to learning rate
- May be slow to adapt
- Requires expert losses
- Memory intensive
- Not suitable for all problems
