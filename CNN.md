# ✏️CNN이란?
## CNN(Convolutional Neural Network, 합성곱 신경망)
- 딥러닝에서 주로 **이미지**나 **영상 데이터**를 처리할 때 사용되는 **대표적인 신경망 구조**입니다.
- **CNN**은 인간의 시각 피질 구조를 모방해 개발되었으며, 이미지의 공간적, 지역적 정보를 효과적으로 학습할 수 있도록 설계되었습니다.

<img width="705" height="287" alt="image" src="https://github.com/user-attachments/assets/a017bec9-5fcf-4a44-b0a6-a77571c1506e" />

---
### 관련 애니메이션
- https://claude.ai/public/artifacts/2cebc728-66b5-414a-9e97-991f60a2a7e1
- https://claude.ai/public/artifacts/a3bda456-4c3f-4127-a921-21ad4c351c98
---
### 등장 배경
- 기존의 인공신경망(특히 다층 퍼셉트론, MLP)은 이미지를 1차원 벡터로 변환해 처리해야 했기 때문에, 이미지의 공간 구조와 지역적 정보가 손실되는 한계가 있었습니다.
- CNN은 이러한 문제를 해결하기 위해 고안되었으며, 이미지의 2차원 구조를 그대로 입력받아 공간 정보를 보존하면서 학습할 수 있습니다.
---
## 주요 구조와 동작 원리
1️⃣ **합성곱 계층**(Convolutional Layer): 입력 데이터에 여러 개의 필터(커널)를 적용해 **특징 맵**(feature map)을 생성합니다. 각 필터는 이미지의 특정 패턴(예: 모서리, 색상 변화 등)을 감지하도록 학습됩니다.

![img](https://github.com/user-attachments/assets/e481c078-c53a-4535-a3b9-a49c2095f3e1) 

✅ **수평엣지**(Horizontal Edge)와 **수직엣지**(Vertical Edge)
- https://claude.ai/public/artifacts/2c09bc56-7cc3-4ea0-b3ca-7678aa107756 <- 관련 애니메이션
- **엣지**란 이미지 내에서 픽셀 값이 급격하게 변하는 경계선을 의미합니다.
- 수평엣지는 위아래 방향(행)으로 픽셀 값이 급격히 변하는 부분, 수직엣지는 좌우 방향(열)으로 픽셀 값이 급격히 변하는 부분을 말합니다.
- 수평엣지 검출 커널

  ```
  -1 -2 -1
   0  0  0
   1  2  1
  ```

<img width="1604" height="808" alt="스크린샷 2025-07-14 10 33 40" src="https://github.com/user-attachments/assets/bb4ad7ff-115f-44e0-8676-bf744880873b" />

- 수직엣지 검출 커널

  ```
  -1  0  1
  -2  0  2
  -1  0  1
  ```

<img width="1604" height="808" alt="스크린샷 2025-07-14 10 33 05" src="https://github.com/user-attachments/assets/f2809c76-4cb6-4591-9294-a1d7ae2b5454" />

---

✅ **블러(Blur Detection)처리**

<img width="1604" height="808" alt="스크린샷 2025-07-14 10 38 08" src="https://github.com/user-attachments/assets/3acaaa77-6a71-44cf-b5c1-d694a8288de3" />


- CNN은 이미지를 입력받아 **블러가 있는지 없는지** 자동으로 분류하거나, **블러의 정도**를 정량적으로 평가하는 데 활용됩니다.
- 기존에는 Laplacian, Sobel 등 엣지 기반 연산자로 블러를 검출했으나, CNN은 **복잡한 패턴을 학습해 더 다양한 블러 유형**(초점 흐림, 모션 블러 등)을 구분할 수 있습니다.

---

<img width="1311" height="885" alt="스크린샷 2025-07-14 11 07 00" src="https://github.com/user-attachments/assets/c94bacdc-23ae-4182-ad54-8d0edae52785" />


---

- 엣지 검출 과정:
1. 이미지를 **흑백**(그레이스케일)으로 변환
2. **노이즈 제거**(필요시)
3. **수평/수직 마스크를 각각 적용**하여 해당 방향의 엣지 검출
4. **두 결과를 합성**하여 전체 엣지(경계선) 추출
- 활용 분야:
1. 객체 인식 및 분할
2. 윤곽선 추출
3. 영상 내 구조 분석
4. 자율주행, 의료 영상 분석 등

<img width="731" height="130" alt="스크린샷 2025-07-14 10 20 31" src="https://github.com/user-attachments/assets/35d414b3-54b2-4d3d-b101-cfb29faef73b" />



---
2️⃣ **활성화 함수**(ReLU 등): 비선형성을 부여해 신경망이 복잡한 패턴을 학습할 수 있도록 합니다. 대표적으로 ReLU(Rectified Linear Unit)가 사용됩니다.

<img width="822" height="362" alt="스크린샷 2025-07-14 09 26 07" src="https://github.com/user-attachments/assets/613ca701-82e5-406a-9d9e-54fef3ca71b9" />

---

3️⃣ **풀링 계층**(Pooling Layer): 특징 맵의 크기를 줄이고, 중요한 정보만 추려내어 연산량을 감소시킵니다. 대표적으로 최대 풀링(max pooling)이 사용됩니다.

<img width="1280" height="497" alt="다운로드" src="https://github.com/user-attachments/assets/f72ab12b-ee2a-43dc-8453-e00106c6e7e2" />

---

4️⃣ **완전 연결 계층**(Fully Connected Layer): 마지막에 위치하며, 추출된 특징을 바탕으로 최종 분류 작업을 수행합니다.

### CNN의 전체적인 네트워크 구조

<img width="640" height="229" alt="image (1)" src="https://github.com/user-attachments/assets/93a051a6-8655-44d0-a6d4-9ff22e37f067" />

### 최신 연구 동향 및 혁신
- 전이 학습(Transfer Learning): 대규모 데이터셋에서 미리 학습된 CNN 모델을 새로운 작업에 재활용하는 방식입니다. 데이터가 적은 환경에서도 높은 성능을 낼 수 있어 실제 응용에서 널리 사용됩니다.
- 경량화 모델: 모바일, IoT 등 자원이 제한된 환경에서도 사용 가능하도록 모델 크기와 연산량을 줄인 EfficientNet, MobileNet 등 경량화 구조가 개발되고 있습니다.
- GAN(Generative Adversarial Networks)과의 결합: CNN과 GAN을 결합해 이미지 생성, 복원, 스타일 변환 등 창의적인 작업에도 활용되고 있습니다.
- 해석 가능성(Explainability): CNN의 의사결정 과정을 설명하고 신뢰도를 높이기 위한 연구가 활발히 진행 중입니다.
- 양자 CNN(QCNN): 양자컴퓨팅의 개념을 접목한 QCNN(Quantum Convolutional Neural Networks)이 등장해, 기존 CNN의 한계를 극복하려는 시도가 이어지고 있습니다.


### 특징 및 장점
- 공간 정보 보존: 이미지의 구조적, 지역적 정보를 유지하며 학습합니다.
- 특징 자동 추출: 사람이 직접 특징을 설계하지 않아도, 네트워크가 학습 과정에서 자동으로 특징을 추출합니다.
- 적은 파라미터: 필터(커널)를 공유함으로써 전체 파라미터 수가 줄어들고, 과적합 위험이 낮아집니다.
- 컴퓨터 비전 분야에 최적화: 이미지 분류, 객체 인식, 얼굴 인식, 자율주행 등 다양한 비전 분야에서 뛰어난 성능을 보입니다.


### 활용 예시
- **이미지 분류**: 고양이/강아지 구분, 손글씨 숫자 인식(MNIST 등)
- **객체 탐지 및 인식**: 자율주행차, CCTV 영상 분석
- **의료 영상 분석**: X-ray, MRI 등에서 이상 부위 탐지


### 한계점 및 최근 동향
- **로컬 특징에 강점**: CNN은 인접한 픽셀 간의 상관관계(로컬 특징)는 잘 추출하지만, 전체 이미지를 아우르는 글로벌 특징 추출에는 한계가 있습니다.
- 최근에는 **트랜스포머(Transformer) 구조**와의 결합 등 다양한 하이브리드 모델이 연구되고 있습니다.
