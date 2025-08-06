
# 🚀 Diabetes Prediction using Logistic Regression and Decision Tree

**Date:** 2025-08-06  
**Author:** Saddam  

---

## 🧠 Problem Statement
Predict whether a patient has diabetes based on diagnostic features using:
- Logistic Regression (Linear model)
- Decision Tree Classifier (Non-linear model)

---

## 📦 1. Import Libraries
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
```
---

## 📥 2. Load the Dataset
```python
df = pd.read_csv("diabetes.csv")
```

---

## 🔍 3. Exploratory Data Analysis (EDA)
```python
print(df.head())
print(df.info())
print(df.describe())
print(df.isnull().sum())

df.hist(bins=20, figsize=(15,10))
plt.tight_layout()
plt.show()

sns.heatmap(df.corr(), annot=True, cmap="coolwarm")
plt.title("Correlation Matrix")
plt.show()

sns.countplot(data=df, x='Outcome')
plt.title("Target Distribution")
plt.show()
```

---

## 🧼 4. Preprocessing
```python
X = df.drop('Outcome', axis=1)
y = df['Outcome']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

---

## 🤖 5. Model 1 - Logistic Regression
```python
lr_model = LogisticRegression()
lr_model.fit(X_train_scaled, y_train)
y_pred_lr = lr_model.predict(X_test_scaled)
print("Accuracy:", lr_model.score(X_test_scaled, y_test))
```

### ➕ Advanced Logistic Regression (L1 Regularization)
```python
final_model = LogisticRegression(C=1, penalty='l1', solver='saga', max_iter=100)
final_model.fit(X_train_scaled, y_train)
final_pred = final_model.predict(X_test_scaled)
print("Accuracy:", final_model.score(X_test_scaled, y_test))
```

---

## 🌳 6. Model 2 - Decision Tree
```python
tree_model = DecisionTreeClassifier()
tree_model.fit(X_train, y_train)
tree_model_pred = tree_model.predict(X_test)
print("Accuracy:", tree_model.score(X_test, y_test))
```

---

## 📈 7. Evaluation Metrics
```python
print("Logistic Regression:
", classification_report(y_test, y_pred_lr))
print("Decision Tree:
", classification_report(y_test, tree_model_pred))
```

---

## 🧪 Bonus - Feature Importance (Tree)
```python
importances = pd.Series(tree_model.feature_importances_, index=X.columns)
importances.sort_values().plot(kind='barh')
plt.title("Feature Importance")
plt.show()
```

---

## 🎯 Key Learnings
- Logistic Regression performed better in our case (~82.4% accuracy).
- Decision Trees are sensitive to overfitting if not tuned.
- Hyperparameter tuning and feature engineering are essential for better results.

---

## 🔥 What's Next?
- Try Random Forest or XGBoost
- Explore ROC-AUC and Precision-Recall curves
- Apply GridSearchCV for hyperparameter tuning

---

*Happy Learning, Keep Building!* 💪
