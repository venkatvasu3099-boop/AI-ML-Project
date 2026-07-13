## Part 2: Supervised Machine Learning Modeling

### Task 1: Label Definitions
*   **Regression Label (`y_reg`):** The continuous column `[Insert Your Target Column]` was chosen as the target for regression.
*   **Classification Label (`y_clf`):** A binary column was derived by binarizing `y_reg` at its median. Values strictly greater than the median were mapped to 1, and values equal to or less than the median were mapped to 0.

### Task 2: Encoding Justification
*   **One-Hot Encoding:** Categorical columns without a natural order were processed using `pd.get_dummies()`, dropping the first column to avoid multicollinearity. One-hot encoding is necessary here because assigning arbitrary integers (label encoding) to nominal categories introduces a false-ordinal relationship. For example, mapping "Red" to 1, "Green" to 2, and "Blue" to 3 would trick the model into mathematically assuming that Blue is "greater than" Red, which is logically false.
*   **Ordinal Encoding:** [If applicable: The column [Name] was mapped to integers because...]

### Task 3: Data Leakage Prevention
The `StandardScaler` was fit **only** on the training features (`X_train`), and then used to transform both `X_train` and `X_test`. If the scaler were fit on the entire dataset prior to splitting, it would constitute data leakage. This is because the scaler calculates the global mean and standard deviation; fitting on the whole dataset allows information about the distribution of the unseen test set to "leak" into the training process, leading to overly optimistic performance estimates.

### Task 4: Regression Analysis
**Coefficients Interpretation:**
*   A **large positive coefficient** means that for every one-unit increase in that *scaled* feature, the predicted target value increases by that coefficient's amount (holding all other features constant).
*   A **large negative coefficient** means that a one-unit increase in the scaled feature is associated with a decrease in the predicted target value by that amount.

**OLS vs. Ridge Comparison Table:**

| Model | MSE | R² |
| :--- | :--- | :--- |
| OLS Linear Regression | [Insert MSE] | [Insert R2] |
| Ridge Regression (alpha=1.0) | [Insert MSE] | [Insert R2] |

**Ridge vs OLS Explanation:**
Ridge regression introduces an L2 penalty term to the cost function, which punishes excessively large coefficients. While OLS minimizes residual error regardless of coefficient size, Ridge forces the model to distribute weight more evenly across correlated features, resulting in a different coefficient profile. The `alpha` parameter controls the strength of this penalty: an `alpha` of 0 is equivalent to standard OLS, while a larger `alpha` forces coefficients closer to zero, reducing model variance at the cost of slight bias.

### Task 5: Classification Analysis
**Imbalance Handling:** 
[State whether you used class_weight='balanced' based on the output of the script].

**Precision and Recall Formulas:**
*   **Precision:**  $$Precision = \frac{TP}{TP + FP}$$
*   **Recall:** $$Recall = \frac{TP}{TP + FN}$$

*(Where TP = True Positives, FP = False Positives, FN = False Negatives)*

**Metric Importance & AUC:**
*   For this specific classification task, **[Choose Precision or Recall]** is more important. [Justify why based on your dataset context. E.g., If false negatives (missing a true event) are highly costly, choose Recall. If false positives (crying wolf) are highly costly, choose Precision.]
*   **AUC Explanation:** The Area Under the ROC Curve (AUC) represents the probability that the model will rank a randomly chosen positive instance higher than a randomly chosen negative one. It measures the model's overall ability to distinguish between the two classes across all possible decision thresholds.

### Task 5b: Decision-Threshold Sensitivity
*   The threshold that maximizes the F1-score on this dataset is **[Insert Threshold]**.
*   To optimize for **[Insert your chosen metric from above, e.g., Recall]**, I would **[lower/raise]** the threshold. The cost of doing so is that while we catch more true positives, we simultaneously increase the number of false positives, which drags down our Precision.

### Task 6: Regularization Experiment
| Model | Precision | Recall | AUC |
| :--- | :--- | :--- | :--- |
| Logistic Reg (C=1.0) | [Insert] | [Insert] | [Insert] |
| Logistic Reg (C=0.01) | [Insert] | [Insert] | [Insert] |

**C Parameter Explanation:**
In logistic regression, the `C` parameter is the *inverse* of regularization strength (similar to $1/\alpha$ in Ridge). A smaller `C` (like 0.01) applies stronger regularization, pushing coefficients closer to zero to prevent overfitting. Reducing `C` to 0.01 **[improved / worsened]** performance on this dataset. [If it worsened: meaning the model was heavily constrained and underfit the data. If improved: meaning it reduced variance and generalized better.]

### Task 7: Bootstrap Confidence Interval
*   **Mean AUC Difference:** [Insert Mean Diff]
*   **95% CI:** [[Insert Lower CI], [Insert Upper CI]]
*   **Conclusion:** The 95% confidence interval **[does / does not]** exclude zero. Because it **[does / does not]** exclude zero, the baseline C=1.0 model's advantage **[is likely consistent across different data samples / may not be reliable]**.
