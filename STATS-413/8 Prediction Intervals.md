[[2024-09-19]] #Regression 

### Prediction Intervals 
Motivation: We know our predicted values $\hat{y}$ are just compromises, best guesses based on some linear approximations. There’s still left over variability that we can’t account for.
- Exhibit: $R^{2}\le 0 \iff RSS > 0 \iff e^{T}e>0$ - residuals is non-zero
- Predicting at an uncertainty range of $[580,620]$ is much different than predicting $[400,800]$

These will be called **prediction intervals**.