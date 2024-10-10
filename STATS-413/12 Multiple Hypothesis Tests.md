[[2024-10-03]] #Regression 

By now, under the **[[4 Hypothesis Tests#^bfc3d4|stronger linear model]]** we know how to
1. Conduct [[4 Hypothesis Tests|hypothesis tests]] for slope coefficients
2. Construct [[7 Inferences|confidence intervals for conditional expectations]]
3. Construct [[8 Prediction Intervals|prediction intervals]] for future observations

These procedures depend upon a parameter, $\alpha$, which is meant to control the [[4 Hypothesis Tests#^1ba045|Type I error]] rate. All is fine when we conduct a **single** hypothesis test / construct a **single** confidence interval. In our tests for outliers, we mentioned how one needs to [[10 Outliers#^9bf0ba|account for the number of hypotheses]] being tested to avoid committing too many Type I errors.

Proper calculation of $p$ -values can be troublesome when we want to simultaneously answer multiple research questions.
- The more you look for something unusual to occur, the more likely you will find it (Data Mining)

```ad-example
**Example**: xkcd Experiment

Suppose I have data on 1000 study participants. 500 of them are randomly assigned to eat no jelly beans for two months. The remaining 500 are split into 20 groups with 25 participants each. In each group, patients are assigned a particular color of jelly bean to consume. They eat 5 jelly beans a day of their assigned color over the course of two months. All 1000 participants count the total number of pimples they’ve had over the course of the study period.

Here, we have 20 hypotheses (for each color) to test for: 
- $H_{0j}:\beta_{j}=0$
- $H_{aj}:\beta_{j}\ne0$
```