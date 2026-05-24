# auto-mpg-analysis
Exploratory Data Analysis (EDA) and visualization of the Auto MPG dataset to uncover technical factors influencing vehicle fuel efficiency.
## Project Overview
This project performs Exploratory Data Analysis (EDA) on the **Auto MPG** dataset. The primary objective is to clean the data, visualize key variables, and uncover the underlying relationships between various vehicle technical specifications (such as weight, horsepower, and cylinders) and their fuel efficiency, measured in Miles Per Gallon (MPG).

## Analysis Objectives
- Handle missing values and standardize data types for accurate processing.
- Analyze the distribution and trends of fuel efficiency (MPG) across different model years and regions of origin.
- Utilize data visualization techniques to identify features with the strongest correlation to MPG.

## Technologies & Libraries Used
- **Environment:** Google Colab
- **Language:** R

## 📊 Key Insights & Model Evaluation

### 1. Primary Drivers of Fuel Efficiency
- **Weight is the Ultimate Penalty:** Vehicle weight (`weight`) is the most highly significant factor (p < 2e-16) negatively impacting fuel efficiency. Every unit increase in weight drastically reduces the MPG, overshadowing other engine specifications.
- **Technological Advancement:** There is a clear, statistically significant evolutionary trend in auto manufacturing. Vehicles produced in the late 70s and early 80s (specifically 1977-1982) show a massive improvement in MPG compared to the early 70s baseline, reflecting the industry's shift towards fuel economy.
- **Regional Superiority:** The region of origin plays a crucial role. Non-US vehicles, specifically from Europe (`origin2`) and Japan (`origin3`), demonstrate statistically significantly higher fuel efficiency compared to American cars, even when controlling for other variables.

### 2. Linear Regression Model Performance
- **High Explanatory Power:** The baseline Multiple Linear Regression model achieved an **R-squared of 0.8815** (Adjusted R-squared: 0.8725). This indicates that approximately 88% of the variance in vehicle fuel consumption can be explained by the selected features.
- **Statistical Significance:** The overall model is highly significant (F-statistic p-value < 2.2e-16).

### 3. Statistical Challenges & Diagnostics
Through rigorous diagnostic testing, several underlying data structures were uncovered:
- **Severe Multicollinearity (VIF Test):** Engine characteristics (`cylinders`, `displacement`, `horsepower`) and `weight` exhibit extremely high Variance Inflation Factors (VIF > 10). This indicates these features are highly entangled, making it difficult to isolate their individual effects and causing features like horsepower to appear statistically insignificant in the presence of weight.
- **Heteroscedasticity (Breusch-Pagan Test):** With a p-value of 8.28e-06, the residuals do not have a constant variance. The model's prediction errors likely scale with the fitted values.
- **Non-Normal Residuals (Jarque-Bera Test):** The residuals deviate from a normal distribution (p < 0.05), suggesting that while the model is highly predictive, the standard confidence intervals for the coefficients might be biased.

### 4. Capturing Non-Linearity with Generalized Additive Models (GAM)
To address the limitations discovered in the linear model—specifically heteroscedasticity and the presence of non-linear relationships—a Generalized Additive Model (GAM) using smoothing splines was implemented. 
- **Non-Linear Significance:** The GAM summary revealed that both `weight` (p < 2e-16) and `horsepower` (p = 0.00192) possess highly significant non-linear relationships with MPG. 
- **Improved Training Fit:** By allowing the continuous variables to form curves rather than rigid straight lines, the GAM achieved a higher explanatory power, explaining 90.6% of the deviance with an Adjusted R-squared of 0.898.

### 5. Final Model Evaluation (Test Set Performance)
To ensure the models' robustness and test for overfitting, both models were evaluated against an unseen testing dataset (20% of the total data). 

| Metric | Linear Regression | Generalized Additive Model (GAM) |
| :--- | :--- | :--- |
| **RMSE** (Lower is better) | 3.131 | **3.020** |
| **R-squared** (Higher is better)| 0.838 | **0.850** |

**Conclusion:** 
The GAM successfully outperformed the traditional Multiple Linear Regression model on unseen data. By accommodating the non-linear trajectories of key predictors like vehicle weight and engine horsepower, the GAM reduced the prediction error (RMSE) and improved the overall fit (R-squared). This proves that fuel efficiency dynamics are intrinsically non-linear, and utilizing advanced modeling techniques like GAM yields more accurate and reliable predictions.
