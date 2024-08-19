---
layout: post
title: "ODEs for influenza forecasting"
date: 2024-04-24
categories: tutorials
permalink: /posts/ode-for-influenza-forecasting/
usemathjax: true
---

```
import numpy as np
import pandas as pd
from scipy.integrate import odeint
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler
from scipy.optimize import minimize

scaler = MinMaxScaler()

def sir_model(y, t, beta, gamma):
    S, I, R = y
    dSdt = -beta * S * I
    dIdt = beta * S * I - gamma * I
    dRdt = gamma * I
    return [dSdt, dIdt, dRdt]
```

load data, todo, have a open available ili data to use (maybe use Michael's repo)


```
ili_data = train_y.reshape(-1, period)
t = np.arange(len(ili_data[0]))
t = (t-t.min())/t.max()-t.min()
```

define the model and fit the data

```
# SIR Model Definition
def sir_model(y, t, beta, gamma):
    S, I, R = y
    dSdt = -beta * S * I
    dIdt = beta * S * I - gamma * I
    dRdt = gamma * I
    return [dSdt, dIdt, dRdt]

# Loss function for parameter estimation
def sir_loss(params, t, data, N):
    beta, gamma = params
    I0 = data[0]
    S0 = N - I0
    R0 = 0
    y0 = [S0, I0, R0]
    solution = odeint(sir_model, y0, t, args=(beta, gamma))
    S, I, R = solution.T
    return np.mean((I - data) ** 2)

N = 1.0  # Normalized population size
initial_params = [0.01, 0.1]  # Initial guess for beta and gamma

# Fit SIR model for each year in the training set
params_per_year = []
years = ili_data.shape[0]
for year in range(years):
    year_data = ili_data[year]
    year_t = np.arange(len(year_data))
    
    result = minimize(sir_loss, initial_params, args=(year_t, year_data, N), bounds=[(0, 1), (0, 1)])
    params_per_year.append(result.x)
    print(f'Year {int(year)}: Estimated parameters: beta={result.x[0]}, gamma={result.x[1]}')

# Visualize the fit for each year
colormap = plt.cm.get_cmap('tab10', years)
fig, axs = plt.subplots(nrows=years, ncols=1, figsize=(10, 2*years))
for year in range(years):
    color = colormap(year)
    year_data = ili_data[year]
    year_t = np.arange(len(year_data))
    beta, gamma = params_per_year[year]
    
    I0 = year_data[0]
    S0 = N - I0
    R0 = 0
    y0 = [S0, I0, R0]
    solution = odeint(sir_model, y0, year_t, args=(beta, gamma))
    S, I, R = solution.T
    
    axs[year].plot(year_t, I, color=color, label=f'Year {int(year+start_year)} Predicted')
    axs[year].scatter(year_t, year_data, color=color, marker='o', label=f'Year {int(year+start_year)} Actual', alpha=0.6)

# fig.legend()
plt.show()

```

Neural ODE

one season

fit one model to multiple season

evaluate on test