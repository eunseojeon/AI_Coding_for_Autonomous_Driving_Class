# Regression(회귀) - KNN 회귀를 사용한 당뇨병 진행 예측
## 목표: 당뇨 환자들 관련 정보로부터 당뇨 진행 정도 예측 

### 1. 모듈 임포트
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsRegressor
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.preprocessing import StandardScaler
```
load_diabetes(): diabetes 데이터셋 로딩 


### 2. Load Dataset
```python
# Load the Diabetes dataset
diabetes = load_diabetes()
X = diabetes.data
y = diabetes.target

# Print dataset description
print(diabetes.DESCR)
```
[Diabetes 데이터셋 정보](https://scikit-learn.org/stable/datasets/toy_dataset.html#diabetes-dataset)

샘플 수: 442  
속성 수: 10  
타겟 값 (예측 값): 진단 1년 뒤 당뇨 진행 상황 (정수)  

속성 목록  
- age  
- sex  
- bmi body mass index
- bp average blood pressure
- s1 tc, total serum cholesterol
- s2 ldl, low-density lipoproteins
- s3 hdl, high-density lipoproteins
- s4 tch, total cholesterol / HDL
- s5 ltg, possibly log of serum triglycerides level
- s6 glu, blood sugar level

### 3. Split the Dataset
```python
# Split the dataset into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### 4. Preprocessing (Normalization)
```python
# Standardize the features
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```
전처리를 통해 학습 데이터와 테스트 데이터를 정규분포로 변환 (정규화)  
### 5. Create and Train the KNN Regressor
```python
# Create and train the KNN regressor
knn_regressor = KNeighborsRegressor(n_neighbors=5)
knn_regressor.fit(X_train, y_train)
```

### 6. Make Predictions
```python
# Make predictions on the test data
y_pred = knn_regressor.predict(X_test)
```

### 7. Evaluate the Model
```
# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f'Mean Squared Error: {mse}')
print(f'R-squared: {r2}')
```
Mean Squared Error: 3047.449887640449  
R-squared: 0.42480887066066253  

### 8. Visualize the Results
```python
# Visualize the results
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_pred, color='blue', label='Predicted vs Actual')
plt.plot([min(y_test), max(y_test)], [min(y_test), max(y_test)], color='red', linewidth=2, label='Ideal fit')
plt.title('KNN Regression: Predicted vs Actual')
plt.xlabel('Actual Disease Progression')
plt.ylabel('Predicted Disease Progression')
plt.legend()
plt.show()
```
![](https://media.geeksforgeeks.org/wp-content/uploads/20240617141430/download-(19).png)
