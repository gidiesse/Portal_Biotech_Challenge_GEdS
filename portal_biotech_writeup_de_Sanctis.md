# Portal Biotech Submission Write-Up

I just wanted to include a brief write-up of what I did in the exercise.

## Step 1 — Data loading and cleaning

I loaded the dataset and tried to clean it and make it ready for use in modelling. I found the data to be clean and without missing values or outliers, so this was fairly straightforward.

## Step 2 — Feature engineering

I performed feature engineering (there was not much feature selection to be done as we only had 3 features). After some research on the relationship between retention time and hydrophobicity, I tried to codify the information given from the peptide sequence by focusing on peptide length and the hydrophobicity associated with each letter using the KD scale. This later turned out to be too much of a condensation, and I also included the frequency with which each letter appears in the peptide sequence as features.

## Step 3 — Linear regression (benchmark)

The classic linear regression model — I wanted this to be my benchmark model as it is the simplest one. The first model did not perform well at all:

- Train R²: 0.518
- Test R²: 0.507

But again I think this is because I condensed the information I had too much. By including the frequency with which each letter appears in the peptide sequence as features, I achieved:

- Train R²: 0.8623
- Test R²: 0.8580
- MAE: 12.85
- Δt95: 66.62

R² is very similar for train and test which is a good sign in terms of the bias-variance trade-off. The model doesn't seem to be overfitting and generalises well to data it hasn't seen before. At 0.858, R² could be a little higher and with more time I would work on perfecting the model to try and improve it.

MAE and Δt95 are both reported, but I am unsure of the unit as retention time in the dataset provided was unitless. Given the prevalence of negative values, I assumed for the purposes of this exercise that it was in the iRT scale, but I am not sure of this. So, we have an MAE of 12.85 units and a Δt95 of 66.62 iRT units. Since we don't have the units, we don't have an absolute interpretation, but rather these metrics are useful to compare between the different models I tested and published benchmarks.

## Step 4 — Ridge and Lasso regression

Given that train and test R² were so similar and that I did not have a huge amount of features, I did not expect much improvement through regularisation. This was in fact the case, and I achieved:

**Ridge**
- Train R²: 0.8623
- Test R²: 0.8580
- MAE: 12.85
- Δt95: 33.31

**Lasso**
- Train R²: 0.8622
- Test R²: 0.8576
- MAE: 12.88
- Δt95: 66.64

With more time, it could have been worth testing different values of alpha to increase the regularisation, but again I do not expect that this would make a huge difference.

## Step 5 — Decision trees

Metrics achieved:

- Train R²: 0.8635
- Test R²: 0.7866
- MAE: 16.74
- Δt95: 85.67

Linear regression outperforms decision trees on the test set by about 0.07. This was not entirely unexpected since decision trees have a tendency to overfit and, given the information in the README that there is a known relationship between molecule size, hydrophobicity, and retention time, it is not difficult to assume that this relationship is linear or close to linear and thus the linear regression model captures this well. Trees would work better if there was a more complex relationship to be found. However, with more time, it would definitely be worth doing some parameter tuning through a grid search and cross-validation. A more studied choice in parameters may lead to better performance, but I would still guess that the linear model is the best choice for this problem.

## Notes on metrics

The metrics chosen to compare the models were the ones that I found to be used in the literature after a quick internet search.
