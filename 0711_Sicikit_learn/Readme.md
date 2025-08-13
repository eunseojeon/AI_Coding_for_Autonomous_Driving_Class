# 20250711_sicikit_learn

## **정리 출처**
- 교수님 ppt [scikit_learn_intro.pptx](./scikit_learn_intro.pptx)

## 1️⃣ 머신러닝 & 데이터 분석
### 머신러닝 기본 개념
- 사람이 직접 프로그래밍하지 않고, **컴퓨터가 데이터를 입력받아** 스스로 학습하는 **인공지능(AI)의 한 분야**입니다.
- 경험을 통해 **자동으로 성능을 개선**하는 컴퓨터 프로그램 연구입니다.
- 참고 정의: "Machine learning is a type of artificial intelligence that allows machines to learn from data without being explicitly programmed.", "The study of computer programs that improve automatically through experience."

<img width="902" height="508" alt="스크린샷 2025-07-11 11 57 42" src="https://github.com/user-attachments/assets/674e216a-fbbe-4484-96af-d47bedfc86b6" />

<img width="807" height="373" alt="image" src="https://github.com/user-attachments/assets/c3b5cacb-0a38-45f8-a5b1-462257f85b0e" />

## 2️⃣ 머신러닝 워크플로우 단계별 요약
1. 데이터 수집: 데이터 소스로부너 학습에 필요한 데이터 확보(라벨링 포함)
2. 데이터 전처리 및 변환: 데이터를 학습에 적합한 형태로 변환
3. 모델 학습: 알고리즘 수행 및 모델 학습, 데이터의 숨겨진 패턴 파악
4. 모델 평가: 학습된 모델의 예측 성능 평가
5. 성능 개선: 모델 최적화를 통한 성능 향상

## 3️⃣ 머신러닝 모델 유형
- **지도 학습**(Supervised learning): 라벨이 매겨진 데이터 (labeled datasets)를 학습하여 주어진 데이터를 분류하거나 결과를 예측
- **비지도 학습**(Unsupervised learning): 라벨이 없는 데이터 (unlablled datasets)를 학습하여  데이터에 숨겨진 패턴이나 구조를 발견
- **강화 학습**(Reinforcement learning): 라벨이 없는 데이터 학습 -> 출력에  대한 평가를  피드백으로 활용 ->  추론 성능 점진적 향상

**Machine learning models**
- 알고리즘을 이용한 학습의  최종 결과물 
- 주어진 데이터로부터 결과를 예측하고 패턴을 식별하기 위한 도구 
 **Machine learning algorithms**
- ML model을 도출하기 위한 기법 
- 공통 목표: 모델 예측 성능 향상

<img width="307" height="212" alt="스크린샷 2025-07-11 13 56 03" src="https://github.com/user-attachments/assets/4894230b-ab89-46c8-8e16-ffdfce25f4fb" />

**Under-and overfitting**
- Fitting: 모델이 학습하는 과정
- Underfitting: 모델이 지나치게 단순하여 데이터에 내재된 패턴을 파악하지 못하는 현상 
- Overfitting: 모델이 지나치게 복잡하여 학습 데이터는 잘 예측하나 새로운 데이터가 입력되면 낮은 성능을 보이는 현상

<img width="409" height="274" alt="image" src="https://github.com/user-attachments/assets/61bc237d-d7a9-4ce2-b694-8ee6742065c7" />

**Advantages of ML-based data analysis**
- 대규모 & 다차원 데이터 처리에 유리함 
- 데이터가 쌓일 수록, 학습이 진행될수록 모델 성능 향상
- 데이터에 숨겨진 패턴 발견에 유리함 

**머신러닝(ML) 프로세스 용어 정리**

<img width="745" height="398" alt="스크린샷 2025-07-11 14 08 17" src="https://github.com/user-attachments/assets/f3914270-017f-4a75-9af3-d44978dd43e1" />

## 4️⃣ Classification(분류) 문제
1. 분류 문제의 개념
- **입력 데이터**(예: 이메일 텍스트, 이미지 픽셀값, 환자 증상 등)를 모델에 넣으면,
- **모델**이 학습을 통해 입력 데이터를 분석하고,
- **결과**로 해당 데이터가 속하는 **카테고리**를 예측합니다.

2. 분류 문제의 특징
- **입력 데이터와 레이블(정답)이 함께 제공**되어야 모델을 학습시킬 수 있습니다. 즉, 데이터에 어떤 클래스(카테고리)가 붙어있는지 라벨링이 필수입니다.
- **평가지표**는 예측된 카테고리의 정확도 등으로 평가합니다.

- 아래 표는 대표적인 분류 문제와 각 문제의 입력(특징)과 출력(클래스)을 정리한 것

<img width="746" height="172" alt="스크린샷 2025-07-11 14 12 13" src="https://github.com/user-attachments/assets/0d39dbcd-0c74-4714-9ac8-6a88358b40d4" />

- Email Spam Detection: 이메일 텍스트와 메타데이터를 입력받아 스팸/비스팸으로 분류
- Digit recognition: 숫자 이미지의 픽셀값을 입력받아 0~9 중 하나로 분류
- Medical diagnosis: 환자 증상과 검사 결과를 입력받아 건강/질병A/질병B 등으로 분류

## 5️⃣ 주요 분류(Classification) 알고리즘
### Support Vector Classifier (SVM)
- 기본 아이디어: 클래스 구분선(decision boundary)과 인접 샘플 간의 거리를 최대화
- 특징: 다차원 데이터 분류에 적합, 비선형 경계도 구분 가능, 과적합 방지, 노이즈에 강함
### 주요 파라미터:
- C(규제 매개변수): 오분류에 대한 페널티
- Kernel(커널 함수): 데이터 분리 방식
- Gamma(커널 계수): 구분선 결정 시 데이터 반영 범위
### k-Nearest Neighbors (KNN)
- 기본 아이디어: 인접한 k개 샘플의 클래스를 참고하여 분류
- 특징: 직관적이고 구현이 간단, 비선형 경계 처리 가능
### 주요 파라미터:
- k: 인접 샘플 수
- Weights: 거리 가중치 적용 방식

<img width="271" height="200" alt="스크린샷 2025-07-11 15 29 31" src="https://github.com/user-attachments/assets/a9cef983-3d5b-4dde-b593-70c1736595a6" />

<img width="255" height="130" alt="스크린샷 2025-07-11 15 30 09" src="https://github.com/user-attachments/assets/9a176079-0256-4c5f-a73f-a9fbd4e489e3" />


## 6️⃣ 분류 성능 평가 지표

<img width="739" height="229" alt="스크린샷 2025-07-11 15 23 18" src="https://github.com/user-attachments/assets/8c8c75f8-e97c-4e8d-a163-b2bfdcf9c61f" />

- **TP**: True Positive, **FP**: False Positive, **FN**: False Negative

## 7️⃣ 회귀(Regression) 문제
- 정의: 입력 데이터를 기반으로 연속적인 값을 예측하는 문제
- 예시:
- - 집값 예측: 면적, 위치, 층수 → 가격
- - 날씨 예측: 온도, 습도, 풍속 → 내일 온도
- - 성적 예측: 출석률, 과제점수 → 성적
### KNN 회귀
- 기본 아이디어: 인접한 k개 샘플 값의 평균을 예측값으로 사용
- 특징: 선형/비선형 데이터 모두 적용 가능, 파라미터 튜닝 용이

## 8️⃣ 실습 데이터셋 및 예제
### 손글씨 숫자 분류 (sklearn.datasets.digits)
- 8x8 픽셀의 흑백 이미지 (총 1,797개 샘플, 10개 클래스)
- 각 이미지의 픽셀값(1D array of 64 values)이 feature, 0~9가 target
### 붓꽃 품종 분류 (sklearn.datasets.load_iris)
- 꽃받침, 꽃잎의 길이와 너비(2 features)
- 150개 샘플, 3개 클래스(setosa, versicolor, virginica)
### 당뇨병 진행 예측 (sklearn.datasets.load_diabetes)
- 442개 샘플, 10개 feature
- Target: 당뇨병 진행 지표(연속적 숫자)

 
## 9️⃣ 머신러닝 용어 정리 표
<img width="739" height="351" alt="스크린샷 2025-07-11 15 26 25" src="https://github.com/user-attachments/assets/a1d18342-8ce7-43aa-bd73-fcb29d40aa32" />


## 🔟 주요 시각 자료(사진/도식) 설명
- 머신러닝 프로세스 도식: 데이터 수집 → 전처리 → 모델 학습 → 평가 → 성능 개선의 순환 구조
- 분류 문제 예시 도식: 입력 데이터(예: 이미지, 텍스트) → 모델 → 예측 결과(클래스)
- Underfitting/Overfitting 시각화:
- - Underfitting: 경계가 너무 단순해 데이터 패턴을 못 잡음
- - Overfitting: 경계가 너무 복잡해 새로운 데이터에 약함


## 1️⃣1️⃣ 영어 용어/개념 한글 해설
- **Supervised Learning**: 지도 학습 (입력과 정답이 함께 제공)
- **Unsupervised Learning**: 비지도 학습 (정답 없이 데이터만 제공)
- **Reinforcement Learning**: 강화 학습 (피드백을 통한 학습)
- **Feature Engineering**: 특징 가공(데이터에서 중요한 특성 추출/변환)
- **Overfitting/Underfitting**: 과적합/과소적합
- **Precision/Recall/F1-score**: 예측 정확도/민감도/조화 평균
