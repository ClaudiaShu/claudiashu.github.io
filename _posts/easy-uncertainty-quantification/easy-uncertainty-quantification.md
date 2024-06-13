---
layout: post
title: "Paper reading: Easy Uncertainty Quantification (EasyUQ)"
date: 2024-06-13
categories: reading
permalink: /posts/easy-uncertainty-quantification/
usemathjax: true
---

<!-- 1. Data Collection and Preparation: -->

<!-- Inputs: Collect pairs of model outputs $$\hat{y}$$ and corresponding ground truth values $$y$$ from the training set. -->

<!-- Outputs: These pairs are used to create a training archive $$D=\{(\hat{y}_i,y_i)\}^N_{i=1}$$. -->

Paper title: Easy Uncertainty Quantification (EasyUQ): Generating Predictive Distributions from Single-Valued Model Output

## Step-by-Step Process of EasyUQ

### 1. Data Collection and Preparation

**Inputs**: Collect pairs of model outputs $$\hat{y}$$ and corresponding ground truth values $$y$$ from the training set.

**Outputs**: These pairs are used to create a training archive $$D = \{(\hat{y}_i, y_i)\}_{i=1}^{N}$$.

### 2. Training Phase

**Step 1: Sorting**:
- Sort the training archive $$D$$ based on the model outputs $$\hat{y}_i$$.
- Let $$\hat{y}_{(1)} \leq \hat{y}_{(2)} \leq \ldots \leq \hat{y}_{(N)}$$ be the sorted model outputs and $$y_{(1)}, y_{(2)}, \ldots, y_{(N)}$$ be the corresponding ground truths.

**Step 2: Isotonic Distributional Regression (IDR)**:
- Apply IDR to the sorted pairs. IDR constructs a non-decreasing sequence of predictive cumulative distribution functions (CDFs) $$\hat{F}(\hat{y})$$ that are consistent with the observed data.
- **Algorithm**: Isotonic regression ensures the fitted CDFs $$\hat{F}(\hat{y}_{(i)}) \leq \hat{F}(\hat{y}_{(i+1)})$$.
- Calculate the CDF values $$\hat{F}(\hat{y}_{(i)})$$ as follows:
  $$
  \hat{F}(\hat{y}_{(i)}) = \frac{1}{N} \sum_{j=1}^{i} \mathbb{1}\{y_{(j)} \leq y_{(i)}\}
  $$
- This step results in a set of calibrated predictive distributions for each model output in the training set.

### 3. Testing Phase

**Step 1: Interpolation**:
- For a new model output $$\hat{y}_{new}$$, interpolate between the nearest calibrated predictive distributions from the training archive.
- **Algorithm**: Use linear interpolation if $$\hat{y}_{new}$$ falls between two training outputs $$\hat{y}_{(i)}$$ and $$\hat{y}_{(i+1)}$$.
  $$
  \hat{F}(\hat{y}_{new}) = \hat{F}(\hat{y}_{(i)}) + \frac{\hat{y}_{new} - \hat{y}_{(i)}}{\hat{y}_{(i+1)} - \hat{y}_{(i)}} (\hat{F}(\hat{y}_{(i+1)}) - \hat{F}(\hat{y}_{(i)}))
  $$

**Step 2: Generate Predictive Intervals or Distributions**:
- From the interpolated CDF $$\hat{F}(\hat{y}_{new})$$, derive prediction intervals (e.g., 95% prediction interval) or full predictive distributions.
- **Algorithm**: To find the $$(\alpha/2)$$ and $$(1 - \alpha/2)$$ quantiles for a 95% prediction interval:
  $$
  \text{Lower Bound} = \hat{F}^{-1}(\alpha/2)
  $$
  $$
  \text{Upper Bound} = \hat{F}^{-1}(1 - \alpha/2)
  $$

## Code example

Prepare a mock data that contains pair of model output:

```python
import numpy as np

# Example data
model_outputs = np.array([2.5, 3.6, 4.1, 5.7, 6.3])
ground_truths = np.array([2.4, 3.8, 4.0, 5.5, 6.1])

# Create training archive
training_archive = list(zip(model_outputs, ground_truths))
print(training_archive)
```

Visualise the data:
```python
# Plot the training data
plt.scatter(model_outputs, ground_truths, color='blue', label='Training Data')
plt.xlabel('Model Outputs')
plt.ylabel('Ground Truths')
plt.title('Training Data')
plt.legend()
plt.show()
```

Sort the pairs:
```python
# Sort the training archive by model outputs
sorted_archive = sorted(training_archive, key=lambda x: x[0])
sorted_model_outputs, sorted_ground_truths = zip(*sorted_archive)
print(sorted_model_outputs)
print(sorted_ground_truths)
```

IDR:
```python
from sklearn.isotonic import IsotonicRegression

# Fit isotonic regression model
ir = IsotonicRegression(out_of_bounds='clip')
cdf_values = ir.fit_transform(sorted_model_outputs, sorted_ground_truths)

# Calculate empirical CDF values
empirical_cdfs = np.array([np.mean(sorted_ground_truths[:i+1] <= sorted_ground_truths[i]) for i in range(len(sorted_ground_truths))])
print(empirical_cdfs)
```

plot:
```python
# Plot the CDF results
plt.step(sorted_model_outputs, empirical_cdfs, where='post', color='red', label='Empirical CDF')
plt.scatter(sorted_model_outputs, empirical_cdfs, color='blue')
plt.xlabel('Model Outputs')
plt.ylabel('CDF Values')
plt.title('CDF of Training Data')
plt.legend()
plt.show()
```

Test:
```python
from scipy.interpolate import interp1d

# Interpolate new model output
new_model_output = 4.8
interpolator = interp1d(sorted_model_outputs, empirical_cdfs, fill_value="extrapolate")
new_cdf_value = interpolator(new_model_output)
print(new_cdf_value)
```

Uncertainty boundary:
```python
# Example of quantile calculation for prediction intervals
alpha = 0.05
lower_bound = np.percentile(empirical_cdfs, 100 * (alpha / 2))
upper_bound = np.percentile(empirical_cdfs, 100 * (1 - alpha / 2))

print(f"95% Prediction Interval: [{lower_bound}, {upper_bound}]")

# Plot the uncertainty boundary
plt.scatter(sorted_model_outputs, empirical_cdfs, color='blue', label='Empirical CDF')
plt.step(sorted_model_outputs, empirical_cdfs, where='post', color='red', label='CDF')
plt.axhline(y=lower_bound, color='green', linestyle='--', label='Lower Bound')
plt.axhline(y=upper_bound, color='purple', linestyle='--', label='Upper Bound')
plt.scatter(new_model_output, new_cdf_value, color='orange', label='New Output CDF')
plt.xlabel('Model Outputs')
plt.ylabel('CDF Values')
plt.title('Uncertainty Boundaries for New Model Output')
plt.legend()
plt.show()
```


## Summary of Algorithms

### Isotonic Regression Algorithm
- **Input**: Sorted pairs $$(\hat{y}_{(i)}, y_{(i)})$$.
- **Output**: Non-decreasing sequence of CDF values $$\hat{F}(\hat{y}_{(i)})$$.
- **Process**: Calculate empirical CDF values and adjust to ensure monotonicity.

### Interpolation Algorithm
- **Input**: New model output $$\hat{y}_{new}$$, sorted training outputs $$\hat{y}_{(i)}$$ and corresponding CDFs $$\hat{F}(\hat{y}_{(i)})$$.
- **Output**: Interpolated CDF value $$\hat{F}(\hat{y}_{new})$$.
- **Process**: Linear interpolation between nearest neighbors in the training archive.

### Predictive Interval Algorithm
- **Input**: Interpolated CDF $$\hat{F}(\hat{y}_{new})$$.
- **Output**: Prediction interval bounds.
- **Process**: Invert the CDF to find the required quantiles.
