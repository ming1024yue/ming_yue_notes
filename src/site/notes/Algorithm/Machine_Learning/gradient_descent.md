---
{"dg-publish":true,"permalink":"/algorithm/machine-learning/gradient-descent/"}
---

```python
import numpy as np

class GradientDescent:
    def __init__(self, learning_rate=0.01, max_iter=1000, tol=1e-6):
        """
        Gradient Descent
        Time Complexity: O(n * d * i)
        Space Complexity: O(d)
        where n is number of samples, d is number of features,
        and i is number of iterations
        """
        self.learning_rate = learning_rate
        self.max_iter = max_iter
        self.tol = tol
        self.weights = None
        self.loss_history = []
    
    def fit(self, X, y):
        n_samples, n_features = X.shape
        self.weights = np.zeros(n_features)
        
        for i in range(self.max_iter):
            # Compute gradient
            gradient = self._compute_gradient(X, y)
            
            # Update weights
            new_weights = self.weights - self.learning_rate * gradient
            
            # Check convergence
            if np.linalg.norm(new_weights - self.weights) < self.tol:
                break
            
            self.weights = new_weights
            self.loss_history.append(self._compute_loss(X, y))
    
    def predict(self, X):
        return X @ self.weights
    
    def _compute_gradient(self, X, y):
        return 2 * X.T @ (X @ self.weights - y) / len(y)
    
    def _compute_loss(self, X, y):
        return np.mean((X @ self.weights - y)**2)

class StochasticGradientDescent(GradientDescent):
    def __init__(self, learning_rate=0.01, max_iter=1000, tol=1e-6, batch_size=32):
        super().__init__(learning_rate, max_iter, tol)
        self.batch_size = batch_size
    
    def fit(self, X, y):
        n_samples, n_features = X.shape
        self.weights = np.zeros(n_features)
        
        for i in range(self.max_iter):
            # Randomly sample batch
            indices = np.random.choice(n_samples, self.batch_size, replace=False)
            X_batch = X[indices]
            y_batch = y[indices]
            
            # Compute gradient
            gradient = self._compute_gradient(X_batch, y_batch)
            
            # Update weights
            new_weights = self.weights - self.learning_rate * gradient
            
            # Check convergence
            if np.linalg.norm(new_weights - self.weights) < self.tol:
                break
            
            self.weights = new_weights
            self.loss_history.append(self._compute_loss(X, y))

class MomentumGradientDescent(GradientDescent):
    def __init__(self, learning_rate=0.01, max_iter=1000, tol=1e-6, momentum=0.9):
        super().__init__(learning_rate, max_iter, tol)
        self.momentum = momentum
        self.velocity = None
    
    def fit(self, X, y):
        n_samples, n_features = X.shape
        self.weights = np.zeros(n_features)
        self.velocity = np.zeros(n_features)
        
        for i in range(self.max_iter):
            # Compute gradient
            gradient = self._compute_gradient(X, y)
            
            # Update velocity
            self.velocity = self.momentum * self.velocity - self.learning_rate * gradient
            
            # Update weights
            new_weights = self.weights + self.velocity
            
            # Check convergence
            if np.linalg.norm(new_weights - self.weights) < self.tol:
                break
            
            self.weights = new_weights
            self.loss_history.append(self._compute_loss(X, y))

class AdamGradientDescent(GradientDescent):
    def __init__(self, learning_rate=0.001, max_iter=1000, tol=1e-6, beta1=0.9, beta2=0.999, epsilon=1e-8):
        super().__init__(learning_rate, max_iter, tol)
        self.beta1 = beta1
        self.beta2 = beta2
        self.epsilon = epsilon
        self.m = None
        self.v = None
        self.t = 0
    
    def fit(self, X, y):
        n_samples, n_features = X.shape
        self.weights = np.zeros(n_features)
        self.m = np.zeros(n_features)
        self.v = np.zeros(n_features)
        
        for i in range(self.max_iter):
            self.t += 1
            
            # Compute gradient
            gradient = self._compute_gradient(X, y)
            
            # Update biased first moment estimate
            self.m = self.beta1 * self.m + (1 - self.beta1) * gradient
            
            # Update biased second moment estimate
            self.v = self.beta2 * self.v + (1 - self.beta2) * gradient**2
            
            # Compute bias-corrected first moment estimate
            m_hat = self.m / (1 - self.beta1**self.t)
            
            # Compute bias-corrected second moment estimate
            v_hat = self.v / (1 - self.beta2**self.t)
            
            # Update weights
            new_weights = self.weights - self.learning_rate * m_hat / (np.sqrt(v_hat) + self.epsilon)
            
            # Check convergence
            if np.linalg.norm(new_weights - self.weights) < self.tol:
                break
            
            self.weights = new_weights
            self.loss_history.append(self._compute_loss(X, y))

# Example usage
if __name__ == "__main__":
    # Generate sample data
    np.random.seed(42)
    n_samples = 100
    n_features = 5
    
    X = np.random.randn(n_samples, n_features)
    true_weights = np.random.randn(n_features)
    y = X @ true_weights + 0.1 * np.random.randn(n_samples)
    
    # Basic Gradient Descent
    gd = GradientDescent(learning_rate=0.01)
    gd.fit(X, y)
    print("Basic Gradient Descent Weights:", gd.weights)
    
    # Stochastic Gradient Descent
    sgd = StochasticGradientDescent(learning_rate=0.01, batch_size=32)
    sgd.fit(X, y)
    print("\nStochastic Gradient Descent Weights:", sgd.weights)
    
    # Momentum Gradient Descent
    mgd = MomentumGradientDescent(learning_rate=0.01, momentum=0.9)
    mgd.fit(X, y)
    print("\nMomentum Gradient Descent Weights:", mgd.weights)
    
    # Adam Gradient Descent
    adam = AdamGradientDescent(learning_rate=0.001)
    adam.fit(X, y)
    print("\nAdam Gradient Descent Weights:", adam.weights)
```

## Explanation
Gradient Descent is an optimization algorithm used to minimize a function by iteratively moving in the direction of steepest descent.

### How it works:
1. Initialize weights randomly
2. Compute gradient of loss function
3. Update weights in direction of negative gradient
4. Repeat until convergence

### Time Complexity:
- Best Case: O(n * d * i)
- Average Case: O(n * d * i)
- Worst Case: O(n * d * i)

### Space Complexity: O(d)

### Applications:
- Linear regression
- Neural networks
- Logistic regression
- Support vector machines
- Deep learning

### Advantages:
- Simple implementation
- Works for convex functions
- Easy to understand
- Widely applicable
- Efficient for large datasets

### Disadvantages:
- May converge slowly
- Sensitive to learning rate
- May get stuck in local minima
- Requires differentiable function
- Memory intensive for large models
