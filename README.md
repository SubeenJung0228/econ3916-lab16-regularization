# Regularization: Buying Bias to Sell Variance

## Objective
I tested whether LASSO and Ridge regression could beat ordinary least squares on a high-dimensional problem where only a few predictors carry real signal.

## Methodology
- I simulated a design with 200 predictors, of which only 5 had a real effect on the response, and 105 training rows.
- I fitted OLS, LASSO and Ridge to the training data and compared their test MSEs.
- I traced the LASSO path to see the order in which the true predictors and the noise variables entered the model.
- I chose the LASSO penalty by 5-fold cross-validation, fitting the scaler inside every fold so that no information from the held-out fold leaked into training.
- I scored LASSO's selected variables against the known truth, counting true predictors found and false positives.
- I gave LASSO and Ridge two near-duplicate predictors to see how each method handles strongly correlated inputs.
- I wrote my own prompt asking an AI to produce the cross-validation loop, ran it for 2 rounds, and checked its chosen lambda and CV MSE against my own results.

## Key Findings
- **Regularization paid off.** Test MSE was 12.9587 for OLS, 10.1128 for Ridge and 1.4219 for LASSO. With far more predictors than the signal needs, OLS overfit badly. Ridge helped some, and LASSO helped the most because it could drop the noise.
- **The LASSO path separated signal from noise early.** All five true predictors were in the model from step 6, and the first noise variable did not enter until step 17.
- **Cross-validation picked a sensible penalty.** The chosen lambda was 0.1995, with a CV MSE of 1.5502.
- **LASSO found everything but over-selected.** It recovered 5 of 5 true predictors, but it also kept 11 false positives at the cross-validated penalty.
- **LASSO and Ridge treat duplicates differently.** Given two near-duplicate predictors, LASSO put 4.462 on one and zero on the other, while Ridge split the weight almost evenly at 1.618 and 1.609.
- **The AI-generated loop matched my results exactly.** All four lambda and CV MSE pairs agreed across both rounds, 0% apart: lambda 0.1995 and CV MSE 1.5502 using all 21 penalties, and lambda 0.1585 and CV MSE 1.5810 using every second penalty.
