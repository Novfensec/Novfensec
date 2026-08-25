# MODULE I: Introduction to Machine Learning

## What is Machine Learning?

Machine Learning is a subfield of Artificial Intelligence where a system learns patterns and relationships directly from data, rather than being explicitly programmed with rules for every situation. In traditional programming, you give the computer rules + data, and it produces output. In ML, you give the computer data + output (examples), and it learns the rules (the model) on its own.

Formally, Tom Mitchell's definition (very commonly asked in exams): *"A computer program is said to learn from experience E with respect to some task T and performance measure P, if its performance at task T, as measured by P, improves with experience E."*

Example: A spam filter (task T = classifying emails) improves (performance P = accuracy) as it sees more labeled emails (experience E = training data).

## Machine Learning Applications

You should be able to name a spread across domains:
- **Healthcare**: disease diagnosis, medical image analysis, drug discovery
- **Finance**: fraud detection, credit scoring, stock prediction
- **E-commerce**: recommendation systems (Amazon, Netflix), demand forecasting
- **NLP**: chatbots, translation, sentiment analysis, spam detection
- **Computer Vision**: face recognition, self-driving cars, object detection
- **Speech**: voice assistants like Siri/Alexa

## Types of Machine Learning

**1. Supervised Learning**
The model learns from labeled data — every training example has an input (features) and a known correct output (label/target). The goal is to learn a mapping function f(X) → Y so that when given new unseen X, it can predict Y accurately.
- Sub-types: Regression (continuous output, e.g., predicting house price) and Classification (discrete/categorical output, e.g., spam or not spam).
- Examples: Linear regression, Logistic regression, SVM, Decision Trees, Naive Bayes, KNN.

**2. Unsupervised Learning**
The model is given data **without labels** and must find hidden structure or patterns on its own.
- Sub-types: Clustering (grouping similar data points, e.g., K-Means), Dimensionality Reduction (e.g., PCA), Association Rule Mining (e.g., market basket analysis).
- Example: Customer segmentation — grouping customers by purchase behavior without knowing predefined categories in advance.

**3. Reinforcement Learning**
An agent interacts with an environment, takes actions, and receives rewards or penalties. The goal is to learn a policy that maximizes cumulative reward over time. Unlike supervised learning, there's no "correct answer" given directly — the agent learns through trial and error.
- Examples: Game playing (AlphaGo, Chess engines), robotics, self-driving car navigation.
- Key terms you might be asked: **Agent, Environment, State, Action, Reward, Policy**.

## Feature Representation, Data Cleaning, Feature Scaling, Feature Engineering

This is a very important practical section — expect at least a short-answer or numerical question here.

**Feature Representation**: Converting real-world data (text, images, categories) into numerical vectors that a machine learning algorithm can process. E.g., converting the color "Red/Green/Blue" into numbers via one-hot encoding: Red = [1,0,0], Green=[0,1,0], Blue=[0,0,1].

**Data Cleaning**: The process of detecting and correcting/removing corrupt, inaccurate, duplicate, or missing data from a dataset. This includes:
- Handling missing values (deletion, mean/median/mode imputation)
- Removing duplicates
- Fixing inconsistent formatting (e.g., "USA" vs "U.S.A")
- Removing outliers

**Feature Scaling**: Since features often have very different ranges (e.g., Age: 0–100 vs Salary: 0–1,000,000), many ML algorithms (especially distance-based ones like KNN, SVM, gradient descent-based ones) perform poorly unless features are scaled to a similar range.

Two main techniques (numerically important!):

1. **Min-Max Normalization**: Scales data to [0,1]
   $$x' = \frac{x - x_{min}}{x_{max} - x_{min}}$$
   
   Example: If Age values range from 20 to 60, and we want to scale Age=35:
   x' = (35-20)/(60-20) = 15/40 = 0.375

2. **Standardization (Z-score normalization)**: Scales data to have mean=0, standard deviation=1
   $$x' = \frac{x - \mu}{\sigma}$$
   
   Example: If mean age μ=40, std dev σ=10, and Age=55:
   x' = (55-40)/10 = 1.5

**Feature Engineering**: The process of creating new, more useful features from raw data to improve model performance. E.g., from a "Date of Birth" column, engineering a new feature "Age," or from "Height" and "Weight," engineering "BMI."

## Supervised and Unsupervised Learning Tasks

- **Regression**: Predicting a continuous numeric value. E.g., predicting house price, temperature, stock price.
- **Classification**: Predicting a discrete category/class label. E.g., spam/not spam, disease/no disease.
- **Clustering**: Grouping data points into clusters based on similarity, without predefined labels. E.g., K-Means clustering customers into segments.

---

# MODULE II: Supervised Learning — Regression

## Types of Regression Models

**1. Simple Linear Regression**
Models the relationship between one independent variable (X) and one dependent variable (Y) as a straight line:
$$Y = \beta_0 + \beta_1 X + \epsilon$$
where β0 = intercept, β1 = slope, ε = error term.

The coefficients are typically found using the **Ordinary Least Squares (OLS)** method, minimizing the sum of squared residuals:
$$\beta_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2}, \quad \beta_0 = \bar{y} - \beta_1 \bar{x}$$

**Worked numerical example:**
X (hours studied): 1, 2, 3, 4, 5
Y (marks): 2, 4, 5, 4, 5

- x̄ = 3, ȳ = 4
- Σ(xi-x̄)(yi-ȳ) = (1-3)(2-4)+(2-3)(4-4)+(3-3)(5-4)+(4-3)(4-4)+(5-3)(5-4)
  = (-2)(-2)+(-1)(0)+(0)(1)+(1)(0)+(2)(1) = 4+0+0+0+2 = 6
- Σ(xi-x̄)² = 4+1+0+1+4 = 10
- β1 = 6/10 = 0.6
- β0 = 4 - 0.6(3) = 4-1.8 = 2.2

So the regression line: **Y = 2.2 + 0.6X**

**2. Multiple Linear Regression**
Extension with multiple independent variables:
$$Y = \beta_0 + \beta_1X_1 + \beta_2X_2 + ... + \beta_nX_n + \epsilon$$
E.g., predicting house price using size, number of rooms, and location together.

**3. Polynomial Regression**
Used when the relationship between X and Y is non-linear (curved). It fits a polynomial equation:
$$Y = \beta_0 + \beta_1X + \beta_2X^2 + \beta_3X^3 + ... + \epsilon$$
Even though the relationship is non-linear in X, it's still "linear" in terms of the coefficients (β), so it's solved using linear regression techniques on transformed features.

## Model Complexity: Overfitting and Underfitting

This is a **very commonly tested** concept.

**Underfitting**: The model is too simple to capture the underlying pattern in data. It performs poorly on both training and test data. This happens due to high bias — the model makes strong, overly simplistic assumptions about the data (e.g., trying to fit a straight line to clearly curved data).

**Overfitting**: The model is too complex and starts memorizing noise/random fluctuations in the training data instead of the actual underlying pattern. It performs extremely well on training data but poorly on unseen test data. This is due to high variance — the model is too sensitive to small fluctuations in training data.

**Bias-Variance Tradeoff**: 
- High Bias + Low Variance = Underfitting
- Low Bias + High Variance = Overfitting
- The goal is to find the sweet spot — a model complex enough to capture true patterns but not so complex it memorizes noise.

Visual intuition (common exam diagram): Plotting model error vs model complexity — training error keeps decreasing as complexity increases, but test error decreases then increases again after a point (that turning point is the optimal complexity).

<img width="700" height="400" alt="image" src="https://github.com/user-attachments/assets/9501b5da-4991-474e-ad9f-9df340fb2857" />

## Evaluation Measures

**R-squared (R²) / Coefficient of Determination**
Measures how much of the variance in the dependent variable is explained by the independent variable(s). Ranges from 0 to 1 (higher = better fit).
$$R^2 = 1 - \frac{SS_{res}}{SS_{tot}} = 1 - \frac{\sum(y_i - \hat{y_i})^2}{\sum(y_i - \bar{y})^2}$$

**Mean Squared Error (MSE)**
Average of squared differences between actual and predicted values. Penalizes larger errors more heavily (due to squaring).
$$MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat{y_i})^2$$

**Mean Absolute Error (MAE)**
Average of absolute differences between actual and predicted values. Treats all errors equally (linear penalty).
$$MAE = \frac{1}{n}\sum_{i=1}^{n}|y_i - \hat{y_i}|$$

**Full worked numerical example (all three together):**

Actual values (y): 10, 20, 30, 40, 50
Predicted values (ŷ): 12, 18, 33, 37, 52

| y | ŷ | error (y-ŷ) | \|error\| | error² |
|---|---|---|---|---|
| 10 | 12 | -2 | 2 | 4 |
| 20 | 18 | 2 | 2 | 4 |
| 30 | 33 | -3 | 3 | 9 |
| 40 | 37 | 3 | 3 | 9 |
| 50 | 52 | -2 | 2 | 4 |

- MAE = (2+2+3+3+2)/5 = 12/5 = **2.4**
- MSE = (4+4+9+9+4)/5 = 30/5 = **6.0**
- RMSE (Root MSE, often asked too) = √6 = **2.449**

For R²: ȳ = 30
SS_tot = (10-30)²+(20-30)²+(30-30)²+(40-30)²+(50-30)² = 400+100+0+100+400 = 1000
SS_res = 30 (from squared errors above)
R² = 1 - 30/1000 = 1 - 0.03 = **0.97** (very good fit)

## Regularization Techniques

Regularization is used to **prevent overfitting** by adding a penalty term to the loss function that discourages the model from assigning too much weight (importance) to any one feature — this keeps the model simpler and more generalizable.

**Ridge Regression (L2 Regularization)**
Adds the sum of squared coefficients as a penalty:
$$\text{Loss} = \sum(y_i-\hat{y_i})^2 + \lambda\sum\beta_j^2$$
- Shrinks coefficients towards zero, but **never exactly zero**.
- Good when you believe most features are useful and want to reduce their influence smoothly, not eliminate them.
- λ (lambda) controls the strength of regularization — larger λ = more shrinkage.

**Lasso Regression (L1 Regularization)**
Adds the sum of absolute values of coefficients as a penalty:
$$\text{Loss} = \sum(y_i-\hat{y_i})^2 + \lambda\sum|\beta_j|$$
- Can shrink some coefficients to **exactly zero**, effectively performing automatic feature selection (removing irrelevant features entirely).
- Preferred when you suspect many features are irrelevant/redundant.

**Elastic Net**
Combines both L1 and L2 penalties:
$$\text{Loss} = \sum(y_i-\hat{y_i})^2 + \lambda_1\sum|\beta_j| + \lambda_2\sum\beta_j^2$$
- Balances feature selection (from Lasso) and coefficient shrinkage (from Ridge). Useful when features are highly correlated with each other (Lasso alone tends to arbitrarily pick one of the correlated features).

**Key exam point**: Ridge = squares the coefficients (never zero); Lasso = absolute value of coefficients (can be zero, does feature selection); Elastic Net = combination of both.

## Real-world Applications of Regression
- Predicting house prices based on size, location, number of rooms
- Sales forecasting based on advertising spend
- Predicting a student's exam score based on hours studied
- Stock price prediction
- Predicting temperature based on humidity, pressure, etc.

---

# MODULE III: Supervised Learning — Classification

## Types of Classification Algorithms
Logistic Regression, SVM, Naive Bayes, K-Nearest Neighbors, Decision Trees, Random Forest, Neural Networks — you should at least be able to name and briefly describe these (the syllabus focuses on the first four).

## Inductive Bias of ML Classifiers

Inductive Bias refers to the **set of assumptions** a learning algorithm uses to predict outputs for inputs it has never seen before. Since a finite training dataset can never fully determine a function's behavior for all possible inputs, every algorithm must make some assumption to generalize — this assumption is its inductive bias.

Example: KNN's inductive bias is that "similar inputs (nearby points) have similar outputs." Linear regression's inductive bias is that "the relationship between input and output is linear." Decision trees assume decision boundaries can be represented via axis-aligned splits.

## Curse of Dimensionality

As the number of features (dimensions) in a dataset increases, the volume of the feature space increases exponentially, causing data points to become increasingly sparse. This creates several problems:
- Distance metrics (like Euclidean distance, crucial for KNN, clustering) become less meaningful because all points start appearing roughly equidistant from each other in high dimensions.
- Requires exponentially more data to maintain the same data density, making models prone to overfitting.
- Increases computational cost.

**Solution approaches**: Dimensionality reduction techniques like PCA (Principal Component Analysis), feature selection, regularization.

## Logistic Regression

Despite the name "regression," this is actually a **classification** algorithm (typically binary classification: 0 or 1).

Instead of directly predicting Y, it predicts the **probability** that Y=1, using the **Sigmoid (logistic) function** to squash any real-valued output into the range [0,1]:
$$P(Y=1|X) = \sigma(z) = \frac{1}{1+e^{-z}}, \quad \text{where } z = \beta_0+\beta_1X_1+...+\beta_nX_n$$

If P(Y=1|X) ≥ 0.5, predict class 1; otherwise predict class 0 (0.5 is the default threshold, but it can be adjusted).

**Numerical example:**
Suppose z = 2 (i.e., β0+β1X1 = 2)
P(Y=1) = 1/(1+e^-2) = 1/(1+0.135) = 1/1.135 ≈ **0.881**

Since 0.881 > 0.5, we classify this as class 1.

**Loss function**: Logistic regression uses **Log Loss (Cross-Entropy Loss)**, not MSE, because MSE would create a non-convex optimization problem with the sigmoid function.

## Support Vector Machines (SVM)

SVM finds the **optimal hyperplane** that best separates data points of different classes. "Optimal" means the hyperplane that maximizes the **margin** — the distance between the hyperplane and the nearest data points from each class (these nearest points are called **Support Vectors**, since they "support"/define the decision boundary).

**Hard Margin SVM**: Assumes data is perfectly linearly separable — no misclassification is allowed at all. Works only when classes don't overlap at all. Very sensitive to outliers (a single misplaced point can drastically change or invalidate the hyperplane).

**Soft Margin SVM**: Allows some misclassification/violations of the margin, using a **slack variable (ξ)** and a regularization parameter **C** that controls the tradeoff between maximizing margin and minimizing classification error.
- Large C → less tolerance for misclassification → narrower margin, closer to hard margin (risk of overfitting)
- Small C → more tolerance for misclassification → wider margin (risk of underfitting)

**Kernel Trick**: When data is **not linearly separable** in its original space, SVM uses kernel functions to implicitly map the data into a higher-dimensional space where it becomes linearly separable — without ever explicitly computing the coordinates in that higher-dimensional space (this makes it computationally efficient).

Common kernels:
- **Linear kernel**: K(x,y) = x·y (no transformation, use when data is already linearly separable)
- **Polynomial kernel**: K(x,y) = (x·y + c)^d
- **RBF (Radial Basis Function) / Gaussian kernel**: K(x,y) = exp(-γ‖x-y‖²) — most commonly used, works well for non-linear boundaries

## Naive Bayes

Based on **Bayes' Theorem**:
$$P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}$$

In classification terms:
$$P(\text{Class}|\text{Features}) = \frac{P(\text{Features}|\text{Class}) \cdot P(\text{Class})}{P(\text{Features})}$$

**"Naive" assumption**: All features are assumed to be **conditionally independent** of each other given the class label. This is rarely true in reality (features are often correlated), but the algorithm still performs surprisingly well in practice — hence "naive."

**Full worked numerical example (classic exam question style):**

Say we want to classify whether to "Play Tennis" based on Weather = "Sunny."

Given:
- P(Play=Yes) = 0.6, P(Play=No) = 0.4
- P(Sunny|Play=Yes) = 0.3, P(Sunny|Play=No) = 0.6
- P(Sunny) = P(Sunny|Yes)P(Yes) + P(Sunny|No)P(No) = 0.3(0.6)+0.6(0.4) = 0.18+0.24 = 0.42

P(Yes|Sunny) = [P(Sunny|Yes)·P(Yes)] / P(Sunny) = (0.3×0.6)/0.42 = 0.18/0.42 ≈ **0.4286**
P(No|Sunny) = (0.6×0.4)/0.42 = 0.24/0.42 ≈ **0.5714**

Since P(No|Sunny) > P(Yes|Sunny), we classify as **"No, don't play tennis"** when it's sunny.

**Types of Naive Bayes classifiers:**
- **Gaussian NB**: Used when features are continuous, assumes features follow a normal (Gaussian) distribution.
- **Multinomial NB**: Used for discrete counts, very common in text classification (e.g., word counts in documents).
- **Bernoulli NB**: Used for binary/boolean features (e.g., word present or absent in a document).

**Applications**: Spam email filtering, sentiment analysis, document/text classification, medical diagnosis.

## K-Nearest Neighbour (KNN)

KNN is a **lazy learning, instance-based** algorithm — it doesn't build an explicit model during training; instead, it stores the entire training dataset and makes predictions at query time by looking at the 'k' closest training points to the new data point, and assigning the majority class among them (for classification) or the average value (for regression).

**Distance Metrics** (very important, numerical questions common):

1. **Euclidean Distance**:
$$d(x,y) = \sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}$$

2. **Manhattan Distance**:
$$d(x,y) = \sum_{i=1}^{n}|x_i-y_i|$$

3. **Minkowski Distance** (generalization of both):
$$d(x,y) = \left(\sum_{i=1}^{n}|x_i-y_i|^p\right)^{1/p}$$
(p=1 gives Manhattan, p=2 gives Euclidean)

**Worked example:**
Point A = (2,3), Point B = (5,7)

Euclidean: √[(5-2)²+(7-3)²] = √[9+16] = √25 = **5**
Manhattan: |5-2|+|7-3| = 3+4 = **7**

**Choosing k:**
- **Small k** (e.g., k=1): Model becomes very sensitive to noise, decision boundary is very jagged/complex → high risk of **overfitting**.
- **Large k**: Model becomes overly smooth, may ignore local patterns and start including points from other classes → risk of **underfitting**.
- **Odd k values** are usually preferred for binary classification, to avoid ties when voting.
- Best practice: use **cross-validation** to systematically test different k values and pick the one with best validation performance.

**Full KNN worked example:**
Training data (2 features), classify a new point (3,3) with k=3:

| Point | X1 | X2 | Class | Distance from (3,3) |
|-------|----|----|-------|------|
| A | 1 | 2 | Red | √(4+1)=√5≈2.24 |
| B | 2 | 3 | Red | √(1+0)=1 |
| C | 5 | 5 | Blue | √(4+4)=√8≈2.83 |
| D | 3 | 1 | Blue | √(0+4)=2 |
| E | 6 | 6 | Blue | √(9+9)=√18≈4.24 |

Sorted by distance: B(1), D(2), A(2.24), C(2.83), E(4.24)
Nearest 3 (k=3): B(Red), D(Blue), A(Red)
Majority vote: Red = 2, Blue = 1 → **Classify as Red**

## Cross-Validation Strategies

Cross-validation is used to evaluate how well a model generalizes to unseen data, and to tune hyperparameters (like k in KNN, or λ in Ridge/Lasso), by splitting the data multiple times into training and validation subsets rather than relying on a single train-test split.

**k-Fold Cross-Validation**: Split the dataset into k equal-sized folds. Train the model on k-1 folds and validate on the remaining 1 fold. Repeat this k times, each time using a different fold as the validation set. Average the k performance scores to get the final estimate. Common choice: k=5 or k=10.

**Leave-One-Out Cross-Validation (LOOCV)**: An extreme case of k-fold where k = n (number of data points). Each iteration trains on all data points except one, and tests on that one left-out point. Very computationally expensive for large datasets but gives an almost unbiased estimate.

**Stratified k-Fold**: Ensures each fold has approximately the same proportion of each class as the full dataset — very important for imbalanced classification problems.

## Evaluation Measures: Confusion Matrix and Related Metrics

This is the **single most important, most numerically-tested topic** in classification. Make sure you fully understand this.

**Confusion Matrix** (for binary classification):

| | Predicted Positive | Predicted Negative |
|---|---|---|
| **Actual Positive** | True Positive (TP) | False Negative (FN) |
| **Actual Negative** | False Positive (FP) | True Negative (TN) |

- **TP**: Correctly predicted positive (e.g., correctly identified disease)
- **TN**: Correctly predicted negative (e.g., correctly identified no disease)
- **FP** (Type I error): Predicted positive, but actually negative (false alarm)
- **FN** (Type II error): Predicted negative, but actually positive (missed detection — often more dangerous, e.g., missing a cancer diagnosis)

**Precision**: Out of all instances predicted as positive, how many were actually positive? Measures how "trustworthy" a positive prediction is.
$$Precision = \frac{TP}{TP+FP}$$

**Recall (Sensitivity / True Positive Rate)**: Out of all actual positive instances, how many did the model correctly identify? Measures how well the model catches positives.
$$Recall = \frac{TP}{TP+FN}$$

**Specificity (True Negative Rate)**: Out of all actual negative instances, how many did the model correctly identify?
$$Specificity = \frac{TN}{TN+FP}$$

**Accuracy**: Overall, what fraction of predictions were correct?
$$Accuracy = \frac{TP+TN}{TP+TN+FP+FN}$$

**F1-Score** (often paired with these, worth knowing even though not explicitly listed): Harmonic mean of precision and recall, useful when you need a balance between the two, especially with imbalanced classes.
$$F1 = \frac{2 \times Precision \times Recall}{Precision + Recall}$$

**Full worked numerical example (classic exam question):**

Suppose a medical test for a disease gives this confusion matrix:
TP = 50, FP = 10, FN = 5, TN = 100

- Accuracy = (50+100)/(50+100+10+5) = 150/165 = **0.909 (90.9%)**
- Precision = 50/(50+10) = 50/60 = **0.833 (83.3%)**
- Recall = 50/(50+5) = 50/55 = **0.909 (90.9%)**
- Specificity = 100/(100+10) = 100/110 = **0.909 (90.9%)**
- F1-Score = 2(0.833×0.909)/(0.833+0.909) = 2(0.757)/(1.742) = 1.514/1.742 = **0.869**

**AUC (Area Under the ROC Curve)**:
The **ROC (Receiver Operating Characteristic) curve** plots **True Positive Rate (Recall)** on the y-axis against **False Positive Rate (1-Specificity)** on the x-axis at various classification threshold settings.

**AUC** is the area under this curve, ranging from 0 to 1:
- AUC = 1.0 → perfect classifier
- AUC = 0.5 → no better than random guessing (diagonal line)
- AUC < 0.5 → worse than random (model is doing something systematically wrong)
- Higher AUC = better ability of the model to distinguish between classes across all thresholds, not just one fixed threshold like accuracy/precision/recall do.

This is why AUC is often preferred over accuracy for imbalanced datasets, since it evaluates performance across the entire range of thresholds rather than a single cutoff.
