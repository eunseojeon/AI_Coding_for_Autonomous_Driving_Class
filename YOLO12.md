# 🧠 YOLO12: 주의 집중 객체 감지
- **YOLO12**는 기존 YOLO 모델에서 사용되던 기존의 CNN 기반 접근 방식에서 벗어나 관심도 중심 아키텍처를 도입했지만, 많은 애플리케이션에 필수적인 실시간 추론 속도는 그대로 유지합니다.
- 이 모델은 주의 메커니즘과 전반적인 네트워크 아키텍처의 새로운 방법론적 혁신을 통해 실시간 성능을 유지하면서 최첨단 물체 감지 정확도를 달성합니다.

#### ✅ Ultralytics 패키지로 객체 감지에 YOLO12를 사용하는 방법 | YOLO12는 빠를까요, 느릴까요 🚀
- https://www.youtube.com/watch?v=mcqTxD-FD5M&t=1s

### ✅ 주요 기능
- **영역 주의 메커니즘**: 대규모 수용 필드를 효율적으로 처리하는 새로운 자체 주의 접근 방식입니다. **피처 맵**을 가로 또는 세로로 동일한 크기의 영역(기본값은 4개)으로 분할하여 복잡한 연산을 피하고 큰 유효 수용 필드를 유지합니다. 따라서 표준 셀프 어텐션에 비해 계산 비용이 크게 절감됩니다.
- **잔여 효율적 계층 집계 네트워크**(R-ELAN): 특히 대규모 주의 집중 모델에서 최적화 문제를 해결하도록 설계된 ELAN 기반의 향상된 기능 집계 모듈입니다. R-ELAN을 소개합니다:
- - 스케일링이 있는 블록 수준의 잔여 연결(레이어 스케일링과 유사).
- - 병목 현상과 같은 구조를 만드는 재설계된 피처 집계 방식입니다.
- **최적화된 주의 집중 아키텍처**: YOLO12는 표준 주의 집중 메커니즘을 간소화하여 효율성과 YOLO 프레임워크와의 호환성을 높였습니다. 여기에는 다음이 포함됩니다:
- - 플래시어텐션을 사용하여 메모리 액세스 오버헤드를 최소화합니다.
- - 더 깔끔하고 빠른 모델을 위해 위치 인코딩을 제거합니다.
- - MLP 비율을 조정(일반적인 4에서 1.2 또는 2로)하면 주의와 피드 포워드 레이어 간의 계산 균형을 더 잘 맞출 수 있습니다.
- - 스택 블록의 깊이를 줄여 최적화를 개선합니다.
- - 계산 효율성을 위해 컨볼루션 연산(적절한 경우)을 활용합니다.
- - 주의 메커니즘에 7x7 분리 가능한 컨볼루션('위치 인식기')을 추가하여 위치 정보를 암시적으로 인코딩합니다.
- **포괄적인 작업 지원**: YOLO12는 객체 감지, **인스턴스 분할, 이미지 분류**, 포즈 추정, 방향성 객체 감지(OBB) 등 다양한 핵심 컴퓨터 비전 작업을 지원합니다.
- **향상된 효율성**: 이전 모델에 비해 더 적은 수의 파라미터로 더 높은 정확도를 달성하여 속도와 정확도 간의 균형이 개선되었습니다.
- **유연한 배포**: 엣지 디바이스에서 클라우드 인프라에 이르기까지 다양한 플랫폼에 배포할 수 있도록 설계되었습니다.

<img width="1920" height="1155" alt="image" src="https://github.com/user-attachments/assets/6ee1375a-f2b0-4fae-86d2-faa162f84bdd" />

### ✅ 지원되는 작업 및 모드
- YOLO12는 다양한 컴퓨터 비전 작업을 지원합니다.
- 아래 표는 작업 지원과 각각에 대해 활성화된 작동 모드(추론, 검증, 훈련 및 내보내기)를 보여줍니다:

| 모델 유형 |	작업	|
|-----|-----|
| [YOLO12](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/12/yolo12.yaml) |	[탐지](https://docs.ultralytics.com/ko/tasks/detect/) |	
| [YOLO12-seg](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/12/yolo12-seg.yaml) |	[세분화](https://docs.ultralytics.com/ko/tasks/segment/)	|
| [YOLO12-pose](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/12/yolo12-pose.yaml) |	[포즈](https://docs.ultralytics.com/ko/tasks/pose/) |	
| [YOLO12-cls](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/12/yolo12-cls.yaml) |	[분류](https://docs.ultralytics.com/ko/tasks/classify/) |	
| [YOLO12-obb](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/models/12/yolo12-obb.yaml) |	[OBB](https://docs.ultralytics.com/ko/tasks/obb/) |


### ✅ 성능 지표
- YOLO12는 모든 모델 규모에서 상당한 **정확도 향상**을 보여주며, 이전의 가장 빠른 YOLO 모델에 비해 속도에서 약간의 트레이드오프가 있습니다.
- 아래는 COCO 검증 데이터 세트에서 **물체 감지**에 대한 정량적 결과입니다:

| 모델 |	크기 (픽셀) |	mAPval 50-95 |	속도 CPU ONNX (ms)	| 속도 T4 TensorRT (ms) |	매개변수 (M) |	FLOPs (B)	| 비교 (지도/속도) |
|---|---|-------|------|-------|-------|-------|-------|
| YOLO12n | 640 |	40.6 |	- |	1.64 |	2.6 |	6.5	| +2.1%/-9%(YOLOv10n 대비) |
| YOLO12s |	640	| 48.0 |	-	| 2.61 |	9.3 |	21.4 |	+0.1%/+42%(RT-DETRv2 대비) |
| YOLO12m |	640 |	52.5 |	- |	4.86 |	20.2 |	67.5 |	+1.0%/-3%(YOLO11m 대비) |
| YOLO12l |	640	| 53.7 |	- |	6.77 |	26.4 |	88.9 |	+0.4%/-8%(YOLO11l 대비) |
| YOLO12x |	640 |	55.2 |	-	| 11.79 |	59.1 |	199.0 |	+0.6%/-4%(YOLO11배 대비) |

- 추론 속도는 TensorRT FP16 **정밀도**를 갖춘 NVIDIA T4 GPU 측정되었습니다.
- 비교는 mAP의 상대적 개선과 속도 변화 비율을 보여줍니다(양수는 더 빠름을, 음수는 더 느림을 나타냄).
- 가능한 경우 YOLOv10, YOLO11 및 RT-DETR 대해 발표된 결과와 비교합니다.

### ✅사용 예
- 이 섹션에서는 YOLO12를 사용한 훈련 및 추론의 예시를 제공합니다.
- 아래 예는 YOLO12 Detect 모델(물체 감지용)에 초점을 맞추고 있습니다.
- 사전 교육 *.pt 모델 ( PyTorch) 및 구성 *.yaml 파일을 YOLO() 클래스를 사용하여 Python 에서 모델 인스턴스를 생성합니다:
```
from ultralytics import YOLO

# Load a COCO-pretrained YOLO12n model
model = YOLO("yolo12n.pt")

# Train the model on the COCO8 example dataset for 100 epochs
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference with the YOLO12n model on the 'bus.jpg' image
results = model("path/to/bus.jpg")
```

### ✅ 주요 개선 사항
1. **향상된 기능 추출**:
- **영역 주의**: 대규모 **수신 필드**를 효율적으로 처리하여 계산 비용을 절감합니다.
- **최적화된 균형**: 주의 집중과 피드 포워드 네트워크 계산 간의 균형이 개선되었습니다.
- **R-ELAN**: R-ELAN 아키텍처를 사용하여 기능 집계를 개선합니다.
2. **최적화 혁신**:
- **잔여 연결**: 특히 대규모 모델에서 훈련을 안정화하기 위해 스케일링과 함께 잔여 연결을 도입합니다.
- **개선된 기능 통합**: R-ELAN 내에서 기능 통합을 위한 개선된 방법을 구현합니다.
- **플래시어텐션**: 플래시어텐션: 메모리 액세스 오버헤드를 줄이기 위해 플래시어텐션을 통합합니다.
3. **아키텍처 효율성**:
- **매개변수 감소**: 이전 모델에 비해 정확도를 유지하거나 개선하면서 더 적은 수의 매개변수를 달성합니다.
- **간소화된 주의**: 간소화된 주의 구현을 사용하여 위치 인코딩을 피합니다.
- **최적화된 MLP 비율**: MLP 비율을 조정하여 컴퓨팅 리소스를 보다 효과적으로 할당합니다.


### ✅ 요구 사항
- 기본적으로 Ultralytics YOLO12 구현에는 플래시어텐션이 필요하지 않습니다.
- 그러나 선택적으로 플래시어텐션을 컴파일하여 YOLO12와 함께 사용할 수 있습니다.
- 플래시어텐션을 컴파일하려면 다음 NVIDIA GPU 중 하나가 필요합니다:
- - **Turing GPU** (예: T4, Quadro RTX 시리즈)
- - **Ampere GPU** (예: RTX30 시리즈, A30/40/100)
- - **Ada 러브레이스 GPU** (예: RTX40 시리즈)
- - **호퍼 GPU** (예: H100/H200)


### ✅ 인용 및 감사
- 연구에 YOLO12를 사용하는 경우 버팔로 대학교와 중국과학원 대학교의 원저를 인용해 주세요:
```@article{tian2025yolov12,
  title={YOLOv12: Attention-Centric Real-Time Object Detectors},
  author={Tian, Yunjie and Ye, Qixiang and Doermann, David},
  journal={arXiv preprint arXiv:2502.12524},
  year={2025}
}

@software{yolo12,
  author = {Tian, Yunjie and Ye, Qixiang and Doermann, David},
  title = {YOLOv12: Attention-Centric Real-Time Object Detectors},
  year = {2025},
  url = {https://github.com/sunsmarterjie/yolov12},
  license = {AGPL-3.0}
}
```

### ✅ YOLO12는 어떻게 높은 정확도를 유지하면서 실시간 물체 감지를 달성할 수 있을까요?
- YOLO12는 속도와 정확성의 균형을 맞추기 위해 몇 가지 주요 혁신 기술을 통합했습니다.
- 영역 주의 메커니즘은 대규모 수신 필드를 효율적으로 처리하여 표준 자체 주의에 비해 계산 비용을 절감합니다.
- 잔여 효율 레이어 집계 네트워크(R-ELAN)는 피처 집계를 개선하여 대규모 주의 집중 모델에서 최적화 문제를 해결합니다.
- 플래시어텐션 사용과 위치 인코딩 제거를 포함한 최적화된 어텐션 아키텍처는 효율성을 더욱 향상시킵니다.
- 이러한 기능을 통해 YOLO12는 많은 애플리케이션에 필수적인 실시간 추론 속도를 유지하면서 최첨단 정확도를 달성할 수 있습니다.

### ✅ YOLO12는 어떤 컴퓨터 비전 작업을 지원하나요?
- YOLO12는 다양한 핵심 컴퓨터 비전 작업을 지원하는 다목적 모델입니다.
- 물체 감지, 인스턴스 분할, 이미지 분류, 포즈 추정, OBB(방향성 물체 감지) 등에서 탁월한 성능을 발휘합니다(자세한 내용 참조).
- 이러한 포괄적인 작업 지원 덕분에 YOLO12는 로봇 공학 및 자율 주행부터 의료 이미징 및 산업 검사에 이르기까지 다양한 애플리케이션을 위한 강력한 도구가 될 수 있습니다.
- 이러한 각 작업은 추론, 검증, 훈련 및 내보내기 모드에서 수행할 수 있습니다.

### ✅ YOLO12는 다른 YOLO 모델 및 RT-DETR 같은 경쟁 제품과 비교했을 때 어떤 점이 다른가요?
- YOLO12는 모든 모델 스케일에서 YOLOv10 및 YOLO11 같은 이전 YOLO 모델에 비해 상당한 정확도 향상을 보여주지만, 가장 빠른 이전 모델에 비해 속도에서 약간의 트레이드오프가 있습니다.
- 예를 들어, YOLO12n은 COCO val2017 데이터 세트에서 YOLOv10n보다 +2.1%, YOLO11n보다 +1.2%의 mAP 개선을 달성했습니다.
- 다음과 같은 모델과 비교했을 때 RT-DETR과 같은 모델과 비교했을 때, YOLO12s는 +1.5%의 mAP 개선과 +42%의 상당한 속도 향상을 제공합니다.
- 이러한 지표는 정확도와 효율성 사이에서 YOLO12의 강력한 균형을 강조합니다. 

### ✅ YOLO12를 실행하기 위한 하드웨어 요구 사항, 특히 플래시어텐션을 사용하기 위한 하드웨어 요구 사항은 무엇인가요?
- 기본적으로 Ultralytics YOLO12 구현에는 플래시어텐션이 필요하지 않습니다.
- 하지만 메모리 액세스 오버헤드를 최소화하기 위해 선택적으로 플래시어텐션을 컴파일하여 YOLO12와 함께 사용할 수 있습니다.
- 플래시어텐션을 컴파일하려면 다음 NVIDIA GPU 중 하나가 필요합니다: 튜링 GPU(예: T4, Quadro RTX 시리즈), 암페어 GPU(예: RTX30 시리즈, A30/40/100), 에이다 러브레이스 GPU(예: RTX40 시리즈) 또는 호퍼 GPU(예: H100/H200).
- 이러한 유연성 덕분에 사용자는 하드웨어 리소스가 허용하는 경우 플래시어텐션의 이점을 활용할 수 있습니다.



