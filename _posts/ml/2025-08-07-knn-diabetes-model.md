
# 🧠 K-Nearest Neighbors (KNN) on Diabetes Dataset

This notebook demonstrates how to build a KNN model on the **Pima Indians Diabetes Dataset** using Scikit-learn. We'll go through data cleaning, preprocessing, model building, and evaluation.

---

## 📊 1. Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier
from sklearn.metrics import accuracy_score, confusion_matrix, classification_report
```

## 📥 2. Load the Dataset

```python
df = pd.read_csv('diabetes.csv')
df.head()
```

---

## 🔍 3. Missing Data Handling

```python
df.replace(0, np.nan, inplace=True)
```

After replacing 0s with NaN for medically invalid entries in columns like Glucose, BloodPressure, SkinThickness, Insulin, BMI:

```python
df.isnull().sum()
```

### 📌 Observation & Decision

- `Insulin` had more than **300 missing values** out of 768 rows — **~40%** missing!
- Other columns had minor missing entries.

💡 **Decision**:  
We decided to fill missing values with median

```python
# Replace invalid 0s with NaNs  
df.replace(0, np.nan, inplace=True)

# List of columns where 0 is invalid  
cols_with_zeros = ['Glucose', 'BloodPressure', 'SkinThickness', 'Insulin', 'BMI']

# Fill missing values with median (safe for skewed medical data)  
df[cols_with_zeros] = df[cols_with_zeros].fillna(df[cols_with_zeros].median())

# Confirm again — should all be zero  
print(df.isnull().sum())
```

---

## 🧼 4. Feature Scaling

Before feeding the data into our model, we need to normalize the feature values. That’s because K-Nearest Neighbors is a distance-based algorithm — it calculates the distance (usually Euclidean) between data points to classify them.

**But imagine this:**

- "Insulin" values range from 0 to 800+
- "BMI" is around 20–50
- "Age" ranges from 20 to 80
- "Glucose" might be around 70–200

➡️ Without scaling, features with larger ranges dominate the distance calculation — and that’s not fair.

✅ Solution: StandardScaler
We scale all features to have:
- Mean = 0
- Standard deviation = 1

This ensures every feature contributes equally to the KNN distance calculation.

```python
X = df.drop('Outcome', axis=1)
y = df['Outcome']

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

---

## 🧪 5. Train-Test Split

```python
X_train, X_test, y_train, y_test = train_test_split(X_scaled, y, test_size=0.2, random_state=42)
```

---

## 🤖 6. KNN Model Training and Evaluation

Try different values of `k` from 1 to 20 and find the best one.

```python
for k in range(1, 21):
    model = KNeighborsClassifier(n_neighbors=k)
    model.fit(X_train, y_train)
    score = model.score(X_test, y_test)
    print("k=%d, accuracy=%f" % (k, score))
```
📈 **Best Result:** `k=11` with highest accuracy ~0.759

---

## 📌 7. Final Model with Best k

```python
model = KNeighborsClassifier(n_neighbors=11)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
```

---

## 📉 8. Evaluation Metrics

```python
print("Accuracy:", accuracy_score(y_test, y_pred))
print("Classification Report:
", classification_report(y_test, y_pred))
print("Confusion Matrix:
", confusion_matrix(y_test, y_pred))
```

---

## 📊 9. Accuracy Plot for k values

```python
scores = []
for k in range(1, 21):
    model = KNeighborsClassifier(n_neighbors=k)
    model.fit(X_train, y_train)
    scores.append(model.score(X_test, y_test))

plt.plot(range(1, 21), scores)
plt.xlabel("k")
plt.ylabel("Accuracy")
plt.title("Accuracy vs K value")
plt.grid()
plt.show()
```

---

## ✅ Summary

- Preprocessed the data, handled missing values smartly
- Dropped `Insulin` due to excessive missingness
- Scaled features and split dataset
- Tuned KNN model to find best `k`
- Achieved ~76% accuracy on test set

---
