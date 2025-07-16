# 📝 YOLO11이란 ?
- https://docs.ultralytics.com/ko/models/yolo11/#key-features
- YOLO11 실시간 물체 감지기의 최신 버전인 Ultralytics YOLO 시리즈의 최신 버전으로, 최첨단 정확도, 속도, 효율성으로 가능성의 한계를 재정의합니다.
- 이전 버전( YOLO )의 인상적인 발전을 바탕으로 아키텍처와 훈련 방법이 크게 개선되어 다양한 컴퓨터 비전 작업에 다용도로 사용할 수 있는 YOLO11 버전이 출시되었습니다.

<img width="3840" height="1680" alt="image" src="https://github.com/user-attachments/assets/79530c8c-1d09-479c-937d-30a63c735f32" />

**🧠 YOLO11의 간략한 설명 영상**
https://www.youtube.com/watch?v=-JXwa-WlkU8

### ✅ 주요 기능
- **향상된 특징 추출**: YOLO11 향상된 백본 및 넥 아키텍처를 채택하여 더욱 정밀한 물체 감지 및 복잡한 작업 수행을 위해 특징 추출 기능을 향상시켰습니다.
- **효율성과 속도에 최적화**: YOLO11 은 정교한 아키텍처 설계와 최적화된 교육 파이프라인을 도입하여 처리 속도를 높이고 정확도와 성능 간의 최적의 균형을 유지합니다.
- **더 적은 파라미터로 더 높은 정확도**: 모델 설계의 발전으로 YOLO11m은 COCO 데이터 세트에서 더 높은 평균 정밀도 (mAP)를 달성하는 동시에 YOLOv8m 보다 22% 적은 수의 매개변수를 사용하여 정확도 저하 없이 계산 효율을 높였습니다.
- 다양한 **환경에서의 적응성**: YOLO11 은 엣지 디바이스, 클라우드 플랫폼, NVIDIA GPU를 지원하는 시스템 등 다양한 환경에 원활하게 배포할 수 있어 유연성을 극대화합니다.
- **광범위한 지원 작업**: 물체 감지, 인스턴스 분할, 이미지 분류, 포즈 추정, 방향성 물체 감지(OBB) 등 다양한 컴퓨터 비전 과제에 대응할 수 있도록 설계되었습니다( YOLO11 ).


### ✅ 지원되는 작업 및 모드
- YOLO11 에서 소개한 다용도 모델 제품군( YOLOv8)을 기반으로 다양한 컴퓨터 비전 작업 전반에 걸쳐 향상된 지원을 제공합니다:

|  모델 |	파일 이름 |	작업 |
|-----|-------|------|
| YOLO11 |	`yolo11n.pt` `yolo11s.pt` `yolo11m.pt` `yolo11l.pt` `yolo11x.pt` |	[탐지](https://docs.ultralytics.com/ko/tasks/detect/) |	
| YOLO11-seg |	`yolo11n-seg.pt` `yolo11s-seg.pt` `yolo11m-seg.pt` `yolo11l-seg.pt` `yolo11x-seg.pt` |	[인스턴스 세분화](https://docs.ultralytics.com/ko/tasks/segment/) |	
| YOLO11-pose |	`yolo11n-pose.pt` `yolo11s-pose.pt` `yolo11m-pose.pt` `yolo11l-pose.pt` `yolo11x-pose.pt`	|[포즈/키포인트](https://docs.ultralytics.com/ko/tasks/pose/) |	
| YOLO11-obb |	`yolo11n-obb.pt` `yolo11s-obb.pt` `yolo11m-obb.pt` `yolo11l-obb.pt` `yolo11x-obb.pt` |	[방향 탐지](https://docs.ultralytics.com/ko/tasks/obb/)	|
| YOLO11-cls |	`yolo11n-cls.pt` `yolo11s-cls.pt` `yolo11m-cls.pt` `yolo11l-cls.pt` `yolo11x-cls.pt` |	[분류](https://docs.ultralytics.com/ko/tasks/classify/) |

- 이 표는 YOLO11 모델 변형에 대한 개요를 제공하며, 특정 작업에서의 적용 가능성과 추론, 검증, 훈련 및 내보내기와 같은 작동 모드와의 호환성을 보여줍니다.
- 이러한 유연성 덕분에 YOLO11 은 실시간 감지부터 복잡한 세분화 작업에 이르기까지 컴퓨터 비전의 광범위한 애플리케이션에 적합합니다.


<img width="807" height="368" alt="스크린샷 2025-07-16 17 05 58" src="https://github.com/user-attachments/assets/c723b013-4bc2-47b1-8348-d0cf5c7d1ade" />

### ✅ 성능
- 탐지

| 모델 |	크기(픽셀) |	mAPval 50-95 |	속도 CPU ONNX(ms) |	속도 T4TensorRT10 (ms) |	매개변수 (M) |	FLOPs (B) |
|-------|--------|----------|----------|---------|--------|----------|
| YOLO11n |	640 |	39.5 |	56.1 ± 0.8 |	1.5 ± 0.0 |	2.6	| 6.5 |
| YOLO11s |	640 |	47.0 |	90.0 ± 1.2 |	2.5 ± 0.0 |	9.4	| 21.5 |
| YOLO11m |	640 |	51.5 |	183.2 ± 2.0 |	4.7 ± 0.1 |	20.1 |	68.0 |
| YOLO11l |	640 |	53.4 |	238.6 ± 1.4 |	6.2 ± 0.1 |	25.3 |	86.9 |
| YOLO11x |	640 |	54.7 |	462.8 ± 6.7 |	11.3 ± 0.2 |	56.9 |	194.9 |

### ✅ 사용 예시
- 아래 예는 YOLO11 객체 감지를 위한 모델감지용 예제입니다.
- PyTorch 사전 교육 *.pt 모델 및 구성 *.yaml 파일을 YOLO() 클래스를 사용하여 Python 에서 모델 인스턴스를 생성합니다:
```
from ultralytics import YOLO

# Load a COCO-pretrained YOLO11n model
model = YOLO("yolo11n.pt")

# Train the model on the COCO8 example dataset for 100 epochs
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference with the YOLO11n model on the 'bus.jpg' image
results = model("path/to/bus.jpg")
```

### ✅ 인용 및 감사
- YOLO11 또는 이 리포지토리의 다른 소프트웨어를 작업에 사용하는 경우 다음 형식을 사용하여 인용해 주세요:
```
@software{yolo11_ultralytics,
  author = {Glenn Jocher and Jing Qiu},
  title = {Ultralytics YOLO11},
  version = {11.0.0},
  year = {2024},
  url = {https://github.com/ultralytics/ultralytics},
  orcid = {0000-0001-5950-6979, 0000-0003-3783-7069},
  license = {AGPL-3.0}
}
```
- DOI는 보류 중이며 사용 가능해지는 대로 인용에 추가될 예정입니다. YOLO11 모델은 아래에 제공됩니다.
- AGPL-3.0 및 엔터프라이즈 라이선스로 제공됩니다.

### ✅  Ultralytics YOLO11 의 주요 개선 사항 (이전 버전과의 비교)
- **향상된 특징 추출**: YOLO11 은 향상된 백본 및 넥 아키텍처를 사용하여 더욱 정밀한 객체 감지를 위해 특징 추출 기능을 향상시켰습니다.
- **최적화된 효율성과 속도**: 정교한 아키텍처 설계와 최적화된 트레이닝 파이프라인으로 정확도와 성능 간의 균형을 유지하면서 더 빠른 처리 속도를 제공합니다.
- **더 적은 파라미터로 더 높은 정확도**: YOLO11m은 YOLOv8m 보다 22% 적은 수의 파라미터로 COCO 데이터 세트에서 더 높은 평균 평균 정밀도 (mAP)를 달성하여 정확도 저하 없이 계산 효율을 높입니다.
- **다양한 환경에서의 적응성**: YOLO11 은 엣지 디바이스, 클라우드 플랫폼, NVIDIA GPU를 지원하는 시스템 등 다양한 환경에 배포할 수 있습니다.
- **광범위한 지원 작업**: YOLO11 은 객체 감지, 인스턴스 분할, 이미지 분류, 포즈 추정, 방향성 객체 감지(OBB) 등 다양한 컴퓨터 비전 작업을 지원합니다.

### ✅ 물체 감지를 위해 YOLO11 모델을 어떻게 훈련하나요?
- 객체 감지를 위한 YOLO11 모델 훈련은 Python 또는 CLI 명령을 사용하여 수행할 수 있습니다.
- 다음은 두 가지 방법에 대한 예제입니다:
#### python
```
from ultralytics import YOLO

# Load a COCO-pretrained YOLO11n model
model = YOLO("yolo11n.pt")

# Train the model on the COCO8 example dataset for 100 epochs
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)
```
#### CLI
```# Load a COCO-pretrained YOLO11n model and train it on the COCO8 example dataset for 100 epochs
yolo train model=yolo11n.pt data=coco8.yaml epochs=100 imgsz=640
```

### ✅ YOLO11 모델은 어떤 작업을 수행할 수 있나요?
- 물체 감지: 이미지 내에서 물체를 식별하고 위치를 파악합니다.
- 인스턴스 세분화: 객체를 감지하고 경계를 묘사합니다.
- [이미지 분류](https://www.ultralytics.com/glossary/image-classification): 이미지를 미리 정의된 클래스로 분류합니다.
- 포즈 추정: 인체의 키포인트를 감지하고 추적합니다.
- OBB(방향성 물체 감지): 회전하는 물체를 감지하여 정밀도를 높입니다.

### ✅ YOLO11 어떻게 더 적은 매개변수로 더 높은 정확도를 달성할 수 있을까요?
- YOLO11 는 모델 설계 및 최적화 기술의 발전을 통해 더 적은 수의 파라미터로 더 높은 정확도를 달성합니다.
- 개선된 아키텍처를 통해 효율적인 특징 추출 및 처리가 가능해져 COCO와 같은 데이터 세트에서 평균 평균 정밀도(mAP)가 높아지는 동시에 YOLOv8m 보다 22% 적은 매개변수를 사용합니다.
- 따라서 YOLO11 은 정확도 저하 없이 계산 효율이 높아져 리소스가 제한된 디바이스에 배포하는 데 적합합니다.

### ✅ YOLO11 을 엣지 디바이스에 배포할 수 있나요?
- 예, YOLO11 은 엣지 디바이스를 포함한 다양한 환경에 적응할 수 있도록 설계되었습니다.
- 최적화된 아키텍처와 효율적인 처리 기능으로 엣지 디바이스, 클라우드 플랫폼, NVIDIA GPU를 지원하는 시스템에 배포하기에 적합합니다.
- 이러한 유연성 덕분에 모바일 디바이스의 실시간 감지부터 클라우드 환경의 복잡한 세분화 작업까지 다양한 애플리케이션에서 YOLO11 을 사용할 수 있습니다. 


