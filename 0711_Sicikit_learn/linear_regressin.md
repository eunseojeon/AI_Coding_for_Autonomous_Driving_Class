# Linear regression 선형회귀

## Data loading and Preparation 데이터 불러오기 및 준비
```python
from sklearn.datasets import load_diabetes
from sklearn.model_selection import train_test_split

X, y = load_diabetes(return_X_y=True)
X = X[:, [2]]  # Use only one feature
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=20, shuffle=False)
```
- 10개 특성 (feature) 중 인덱스 2에 위치한 feature만 사용  
- 결과: x 형태가 (442, 10) -> (442, 1)로 변경   
- 학습/테스트 데이터 분할 (테스트 데이터 갯수 = 20)
- X_train: (422, 1)
- X_test : (20, 1) 
- y_train: 길이 422 벡터 
- y_test : 길이 20 벡터 

## Linear regression model 생성
```python
from sklearn.linear_model import LinearRegression

regressor = LinearRegression().fit(X_train, y_train)
```

## Model evaluation (모델 평가)
```python
from sklearn.metrics import mean_squared_error, r2_score

y_pred = regressor.predict(X_test)

print(f"Mean squared error: {mean_squared_error(y_test, y_pred):.2f}")
print(f"Coefficient of determination: {r2_score(y_test, y_pred):.2f}")
```
- 평가지표: MSE: 평균 제곱 오차, R2 score: 모델이 전체 변동을 얼마나 잘 설명하는지를 나타냄 (1에 가까울 수록 데이터를 잘 설명하는 것, 0 이하는 평균값 예측보다 못 함)
- 결과
  ```
  Mean squared error: 2548.07
  Coefficient of determination: 0.47
  ```
## 결과 그래프 그리기
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(ncols=2, figsize=(10, 5), sharex=True, sharey=True)

ax[0].scatter(X_train, y_train, label="Train data points")
ax[0].plot(
    X_train,
    regressor.predict(X_train),
    linewidth=3,
    color="tab:orange",
    label="Model predictions",
)
ax[0].set(xlabel="Feature", ylabel="Target", title="Train set")
ax[0].legend()

ax[1].scatter(X_test, y_test, label="Test data points")
ax[1].plot(X_test, y_pred, linewidth=3, color="tab:orange", label="Model predictions")
ax[1].set(xlabel="Feature", ylabel="Target", title="Test set")
ax[1].legend()

fig.suptitle("Linear Regression")

plt.show()
```

![](https://scikit-learn.org/stable/_images/sphx_glr_plot_ols_ridge_001.png)

위 실행 결과로부터 알 수 있는 것: single-feature만을 이용한 linear regression은 오차 관점에서 낮은 성능을 보임
## Ordinary Least Squares and Ridge Regression Variance
