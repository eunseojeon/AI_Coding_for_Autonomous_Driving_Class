# 텐서알티(TensorRT)와 파이토치(PyTorch) 비교

<img width="1191" height="552" alt="img" src="https://github.com/user-attachments/assets/5681b651-1ebe-44f6-bf5d-43df3d24a7d8" />

![다운로드](https://github.com/user-attachments/assets/5b3f0d18-d048-4725-8b9d-651b92aed279)

## 핵심 용도와 차이

- **파이토치(PyTorch)**  
  - **종류:** 딥러닝 프레임워크  
  - **주요 기능:** 학습(Training)과 추론(Inference) 모두 지원  
  - **설명:**  
    - 파이썬 기반 오픈소스 딥러닝 프레임워크로, 신경망 설계, 모델 학습 및 평가, 다양한 AI 연구와 개발에 활용
    - 동적 연산그래프(Define-by-run) 방식으로 사용이 쉽고 직관적
    - 연구와 프로토타이핑, 실제 서비스 모두에서 폭넓게 활용됨
    - 모델 설계, 학습 데이터 처리, 커스텀 Layer, 다양한 하드웨어 지원 등 "개발자 친화성"이 높음

- **텐서알티(TensorRT)**
  - **종류:** 딥러닝 추론 엔진(모델 최적화 라이브러리)
  - **주요 기능:** 학습된 딥러닝 모델을 **추론(inference)**에 최적화하여 속도를 **수배~수십배 가속**  
  - **설명:**  
    - NVIDIA가 개발한 GPU 추론 최적화 소프트웨어
    - 사전에 학습된 파이토치, 텐서플로우 등 모델을 가져와 **추론에 특화된 최적화** 적용  
    - FP16, INT8 등 저정밀도 연산 적용, 그래프 최적화, 레이어 병합 등 하드웨어 맞춤 최적화 기법 제공  
    - 실제 서비스(자율주행, 실시간 스트리밍, 서버, 임베디드 등)에 "빠른 추론"이 필수일 때 사용  
    - 파이토치 모델을 ONNX 등으로 변환한 뒤 TensorRT로 최적화하는 방식이 일반적


## 주요 비교

| 항목                | 파이토치(PyTorch)                                       | 텐서알티(TensorRT)                              |
|---------------------|--------------------------------------------------------|-------------------------------------------------|
| 목적/역할           | 딥러닝 모델 "학습과 추론" 전반                         | 학습된 모델의 추론 속도 최적화/가속화           |
| 지원 플랫폼         | 다양한 하드웨어(GPU, CPU 등)                           | NVIDIA GPU (특화)                               |
| 활용 단계           | 연구, 개발, 학습, 추론, 프로토타입, 프로덕션           | **추론(서비스 배포 시)**에 특화                  |
| 사용 방식           | 모델 정의, 학습 데이터 준비, 학습, 평가, 저장           | 이미 학습된 모델을 받아 최적화·추론 엔진 변환    |
| 주요 강점           | 동적 그래프, 직관적인 코드, 높은 유연성, 광범위한 지원 | 추론 성능 가속(FP16, INT8, layer fusion 등), TCO 절감|
| 통합/연동           | (직접 추론 가능, Torch-TensorRT 등으로 최적화 가능)    | 파이토치/TensorFlow/ONNX 등 다양한 훈련 모델 지원 |
| 학습(Training) 지원 | 가능                                                | 불가(단, 추론만 가능)                            |



## ❗️ TensorRT 예제 코드 및 설명

```python
import tensorrt as trt
import pycuda.driver as cuda
import pycuda.autoinit
import numpy as np

TRT_LOGGER = trt.Logger(trt.Logger.WARNING)

def build_engine(onnx_file_path):
    with trt.Builder(TRT_LOGGER) as builder, builder.create_network(1) as network, trt.OnnxParser(network, TRT_LOGGER) as parser:
        builder.max_batch_size = 1
        builder.max_workspace_size = 1 << 30  # 1GB workspace
        
        with open(onnx_file_path, 'rb') as model:
            parser.parse(model.read())
        
        engine = builder.build_cuda_engine(network)
        return engine

def infer(engine, input_data):
    context = engine.create_execution_context()
    d_input = cuda.mem_alloc(input_data.nbytes)
    d_output = cuda.mem_alloc(input_data.nbytes)  # assuming output size = input size for simplicity

    stream = cuda.Stream()
    cuda.memcpy_htod_async(d_input, input_data, stream)
    context.execute_async(bindings=[int(d_input), int(d_output)], stream_handle=stream.handle)
    output_data = np.empty_like(input_data)
    cuda.memcpy_dtoh_async(output_data, d_output, stream)
    stream.synchronize()
    return output_data

# ONNX 모델을 TensorRT 엔진으로 변환하여 추론하는 예제
onnx_model_path = "model.onnx"
input_data = np.random.rand(1, 3, 224, 224).astype(np.float32)  # 예시 입력
engine = build_engine(onnx_model_path)
output = infer(engine, input_data)
print("TensorRT inference output shape:", output.shape)
```

**설명:**

- **TensorRT**는 NVIDIA에서 제공하는 딥러닝 추론 최적화 라이브러리로, 주로 ONNX 모델을 TensorRT 엔진으로 변환해 GPU에서 최적화된 추론을 빠르게 수행합니다.
- 위 코드는 ONNX 모델 파일을 읽어 TensorRT 엔진을 빌드하고, CUDA 메모리에 데이터를 옮겨 GPU에서 추론을 실행하는 예입니다.
- TensorRT는 최적화된 연산과 INT8, FP16 정밀도 지원으로 추론 속도를 크게 향상시키는 데 초점을 둡니다.

---

## ❗️ PyTorch 예제 코드 및 설명

```python
import torch
import torch.nn as nn
import torch.optim as optim

# 간단한 선형 모델 정의
class SimpleModel(nn.Module):
    def __init__(self):
        super(SimpleModel, self).__init__()
        self.linear = nn.Linear(10, 1)  # 입력 10차원, 출력 1차원
    
    def forward(self, x):
        return self.linear(x)

# 모델, 손실함수, 옵티마이저 정의
model = SimpleModel()
criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.01)

# 더미 데이터
inputs = torch.randn(100, 10)
targets = torch.randn(100, 1)

# 학습 1 에폭 (예시)
model.train()
optimizer.zero_grad()
outputs = model(inputs)
loss = criterion(outputs, targets)
loss.backward()
optimizer.step()

print("PyTorch training loss:", loss.item())
```

**설명:**

- PyTorch는 딥러닝 연구 및 개발을 위한 프레임워크로, 동적 계산 그래프와 직관적인 인터페이스를 제공하여 모델 설계, 학습, 추론을 쉽게 할 수 있습니다.
- 위 코드는 간단한 선형 회귀 모델을 정의하고, 임의의 입력/출력 데이터에 대해 1회 학습하는 기본 예입니다.
- TensorRT와 달리 PyTorch는 주로 모델 설계와 학습에 최적화되어 있으며, 연구 및 프로토타이핑에 널리 사용됩니다.

***

## 요약

| 구분          | TensorRT                             | PyTorch                           |
|---------------|------------------------------------|----------------------------------|
| 주요 목적     | 고성능 딥러닝 추론(추론 최적화)   | 딥러닝 모델 설계, 학습, 추론 지원|
| 입력 형식     | 주로 ONNX 모델                     | PyTorch 자체 모델 구조           |
| 실행환경     | NVIDIA GPU 최적화                   | CPU, GPU 모두 지원               |
| 사용성        | 주로 최적화 및 배포용               | 연구, 개발, 실험용               |
| 코드 예제     | ONNX 모델 불러와 엔진 빌드 및 추론 | 모델 정의 및 학습 수행           |

- **TensorRT** 코드는 ONNX 모델을 변환해 추론을 고속 수행하는 데 초점이 있으며,
- **PyTorch** 코드는 모델을 직접 정의하고 학습시키는 데 초점이 있습니다.
- 각기 다른 목적에 최적화된 도구임을 이해하면 비교할 때 도움이 됩니다.


## 요약

- **파이토치**는 딥러닝 모델을 설계·학습·평가·추론까지 전 과정을 담당하는 프레임워크
- **텐서알티**는 학습 완료된 모델을 빠르고 효율적으로 **실서비스에 투입(추론 속도 극대화)**하기 위한 성능 가속화 도구
- 연구·개발 단계에선 파이토치, 실서비스(특히 NVIDIA GPU 기반)에선 텐서알티로 배포 및 가속을 담당하며 **상호보완적**임

