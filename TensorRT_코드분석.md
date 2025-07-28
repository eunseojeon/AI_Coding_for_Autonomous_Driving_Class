# 📝 TensorRT 코드 분석
- [교수님께서 주신 코드](0728_TensorRT.ipynb)

### 메인 코드를 실행시켰더니 파일이 존재하지 않는다고 떠서 추가한 코드
```
import os
os.makedirs('/content/dataset', exist_ok=True)
# 만약 /content/dataset 폴더가 없다면 새로 만듭니다.
```
---

### 여러번 실패해서 생긴 파일 때문에 헷갈릴 때, 파일을 삭제하려고 쓰는 코드
```
import shutil
import os

folder = "/content"
for filename in os.listdir(folder):
    file_path = os.path.join(folder, filename)
    try:
        if os.path.isfile(file_path) or os.path.islink(file_path):
            os.unlink(file_path)  # 파일 또는 심볼릭 링크 삭제
        elif os.path.isdir(file_path):
            shutil.rmtree(file_path)  # 폴더(디렉토리) 삭제
    except Exception as e:
        print(f'Failed to delete {file_path}. Reason: {e}')

#위 코드는 아래 코드들에서 오류났을 때, 새로운 파일이 생기니까 그 파일을 다 삭제해주는 코드! (리셋느낌)
```
---

### Zip 파일을 이용해야 하는데 없어서 추가해주기
```
from google.colab import files
uploaded = files.upload()
#파일 업로드 (dataset.zip파일을 업로드해야함)
```
---

### 압축된 파일을 해제해줌
```
!unzip /content/dataset.zip -d /content/dataset/
#압축해제
```
- 아래 사진은 Zip파일 안에 들어있는 것

<img width="384" height="168" alt="스크린샷 2025-07-28 18 26 23" src="https://github.com/user-attachments/assets/6eda592d-fbcd-4f94-925f-3e9c26e95cc0" />

- 처음에 이미지가 포함되어 있는 이 Zip파일을 넣는 걸 몰라서 계속 오류가 뜸 -> **Zip파일 꼭 넣어주기!**
---

### 🚗 유튜브 영상 다운로드 후 객체 인식해주는 영상 저장
#### ✏️ YOLO 구현체를 쉽게 사용할 수 있게 해주는 라이브러리
```
!pip install ultralytics yt-dlp #YOLO 구현체를 쉽게 사용할 수 있게 해주는 라이브러리

from ultralytics import YOLO
import glob
import cv2
import numpy as np
from IPython.display import Video
import shutil
import time
```

#### ✏️ yaml 수정 (핵심 문제 해결)
```
yaml_fix = '''path: /content/dataset
train: train/images
val: valid/images
names:
  0: lane
  1: traffic_sign
nc: 2'''
```
- 내가 가지고 있는 dataset.yaml 파일도 visual studio로 들어가서 저렇게 수정해주기

<img width="617" height="244" alt="스크린샷 2025-07-28 18 23 22" src="https://github.com/user-attachments/assets/c3cf695e-6634-4c82-b3f6-5b6701c081e9" />

#### ❗️ 파일을 'dataset.yaml' 이름으로 저장하여 나중에 모델 평가 때 사용
```
with open('/content/dataset.yaml', 'w') as f:
    f.write(yaml_fix) #이 파일을 'dataset.yaml' 이름으로 저장하여 나중에 모델 평가 때 사용

print("🚀 TensorRT 최적화 YOLO 추론 시작!")
print("="*60)
```

#### 1️⃣ 기본 모델들 로드
```
print("🤖 기본 모델 로드 중...")
base_model = YOLO('yolo11n.pt') # 미리 학습된 기본 YOLO 모델
custom_model = YOLO('/content/dataset/best.pt') # 내가 학습시킨 커스텀 모델

print(f"기본 모델 클래스 수: {len(base_model.names)}")
print(f"커스텀 모델 클래스 수: {len(custom_model.names)}")
```
#### 2️⃣ TensorRT로 변환
```
print("\n⚡ TensorRT 변환 중...")
print("기본 모델 → TensorRT 변환...")
# base_model.export(format='engine', half=True, device='cpu')  # FP16 최적화. cpu는 돌아가지 않아서 주석처리 한 것.
base_model.export(format='engine', half=True, device=0)  # 또는 device='cuda:0'
base_trt_path = 'yolo11n.engine'

print("커스텀 모델 → TensorRT 변환...")
#custom_model.export(format='engine', half=True, device='cpu') # 이것도 cpu는 돌아가지 않아서 주석처리 한 것
custom_model.export(format='engine', half=True, device=0)  # 또는 device='cuda:0'
custom_trt_path = '/content/dataset/best.engine'
```
- `export()` 함수로 PyTorch YOLO 모델을 TensorRT 엔진 파일(.engine)로 변환
- `format='engine'`은 TensorRT를 의미하며, NVIDIA GPU에서 고속 추론을 위해 사용하는 형식입니다.
- `half=True`는 FP16(16-bit) 연산을 써서 GPU 속도를 높이는 최적화 옵션.
- `device=0`은 GPU 0번 장치를 사용한다는 뜻, 반드시 GPU 환경에서만 가능하며 CPU는 지원 안 됩니다.
- 이렇게 변환하면 **원본 PyTorch 모델 대비 추론 속도가 크게 빨라집**니다.


#### 3️⃣ TensorRT 모델 로드
```
print("\n🔥 TensorRT 모델 로드 중...")
base_trt_model = YOLO(base_trt_path)
custom_trt_model = YOLO(custom_trt_path) #이렇게 .engine으로 변환된 TensorRT 모델 파일을 다시 불러옵니다. 이제 이 모델들로 추론을 할 수 있음

print("✅ TensorRT 모델 로드 완료!")
```

#### 4️⃣ 유튜브 영상 다운로드 및 경로 찾기
```
print("\n📥 YouTube 영상 다운로드 중...")
!yt-dlp -f 'best[height<=720]' -o '/content/test_video.%(ext)s' 'https://www.youtube.com/watch?v=AxLmroTo3rQ'

video_path = glob.glob('/content/test_video.*')[0]
print(f"✅ 다운로드 완료: {video_path}")
```

#### 5️⃣ 성능 비교 함수
```
def performance_comparison(video_path, frames_to_test=100):
    """PyTorch vs TensorRT 성능 비교""" #첫 100 프레임 정도에 대해 각각 PyTorch 기본 + 커스텀 모델, TensorRT 기본 + 커스텀 모델로 추론 시간을 측정

    print(f"\n⏱️ 성능 비교 (첫 {frames_to_test}프레임)")
    print("-" * 50)

    cap = cv2.VideoCapture(video_path) #OpenCV로 video_path 경로에 있는 비디오를 읽을 준비. 한 프레임씩 꺼내 쓸 수 있게 객체(cap)를 만듦

    # PyTorch 모델 성능 테스트
    pytorch_times = []
    for i in range(frames_to_test):
        ret, frame = cap.read() #영상에서 한 장씩 꺼내서
        if not ret:
            break

        #기본 YOLO 모델(base_model)과 커스텀 YOLO 모델(custom_model)로 객체 인식을 각각 해봅니다.
        start_time = time.time()
        _ = base_model(frame, verbose=False)
        _ = custom_model(frame, verbose=False)
        end_time = time.time() #time.time()으로 각 프레임 처리(두 모델 합친 시간) 걸린 시간을 측정해, 리스트에 담아둠

        pytorch_times.append(end_time - start_time) #즉, 원본 PyTorch 기반 모델 두 개의 속도를 "한 번에 측정"

    # TensorRT 모델 성능 테스트
    cap.set(cv2.CAP_PROP_POS_FRAMES, 0)  # 영상을 처음으로 되돌리기
    # 위에서 100장을 이미 읽었으니, 다시 동영상 처음으로 돌려서 tensorRT 모델에서 동일한 프레임을 사용하게함

        tensorrt_times = [] #추론 속도 측정
        for i in range(frames_to_test):
            ret, frame = cap.read()
            if not ret:
              break

        start_time = time.time()
        _ = base_trt_model(frame, verbose=False)
        _ = custom_trt_model(frame, verbose=False)
        end_time = time.time()

        tensorrt_times.append(end_time - start_time)
        #최적화된 TensorRT 모델 두 개(base_trt_model, custom_trt_model)로 같은 영상을 처리·시간 측정

    cap.release() #더이상 영상에서 프레임을 안 쓸 것이니, 자원 정리

    # 결과 출력
    pytorch_avg = np.mean(pytorch_times) * 1000  # ms로 변환. np.mean()은 평균값 구하기.
    tensorrt_avg = np.mean(tensorrt_times) * 1000 #각각 PyTorch, TensorRT 환경에서 1프레임 처리 평균 시간(밀리초) 반환
    speedup = pytorch_avg / tensorrt_avg #TensorRT가 빨라지면 speedup이 2이상~10이상(또는 그 이상) 나옴.

    print(f"🐍 PyTorch 평균: {pytorch_avg:.2f}ms/frame ({1000/pytorch_avg:.1f} FPS)")
    print(f"⚡ TensorRT 평균: {tensorrt_avg:.2f}ms/frame ({1000/tensorrt_avg:.1f} FPS)")
    print(f"🚀 속도 향상: {speedup:.2f}x")
    # 1,000ms를 평균 프레임 처리시간(ms)으로 나누면 초당 프레임 처리(FPS)입니다.
    # 예를 들면 50ms면 20FPS(1,000/50=20).

    return speedup #결과값 반환

# 성능 비교 실행
speedup_ratio = performance_comparison(video_path)
```

#### 6️⃣ TensorRT 최적화된 결합 추론
```
def tensorrt_combined_inference(video_path, output_path='/content/tensorrt_result.mp4'):
    """TensorRT 최적화된 결합 추론"""

    cap = cv2.VideoCapture(video_path) #cv2(OpenCV)로 동영상 파일을 "한 장씩" 읽을 수 있게 엽니다.
    fps = int(cap.get(cv2.CAP_PROP_FPS))
    width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
    total_frames = int(cap.get(cv2.CAP_PROP_FRAME_COUNT))
    # fps, width, height, total_frames: 원본 동영상의 프레임 속도, 크기, 총 프레임수 정보를 저장합니다.
    # cv2.VideoWriter: 처리된 프레임들을 모아서 새 동영상을 만들어 저장할 준비를 합니다.

    # 출력 영상 설정
    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    out = cv2.VideoWriter(output_path, fourcc, fps, (width, height))

    print(f"\n🎬 TensorRT 최적화 영상 처리 중... (총 {total_frames} 프레임)")

    #루프 시작(전체 프레임 반복)
    frame_count = 0
    total_inference_time = 0

    while True:
        ret, frame = cap.read() #한 프레임을 꺼냄
        if not ret:
            break

        # TensorRT 추론 (시간 측정)
        start_time = time.time()

        # 기본 TensorRT 모델 추론
        base_results = base_trt_model(frame, verbose=False)

        # 커스텀 TensorRT 모델 추론
        custom_results = custom_trt_model(frame, verbose=False)

        inference_time = time.time() - start_time
        total_inference_time += inference_time

        # 결과 시각화
        annotated_frame = frame.copy()

        # 기본 YOLO 결과 그리기 (파란색 박스)
        if base_results[0].boxes is not None:
            for box in base_results[0].boxes:
                x1, y1, x2, y2 = map(int, box.xyxy[0])
                conf = float(box.conf[0])
                cls = int(box.cls[0])

                if conf > 0.3: #conf: 신뢰도(0~1), 0.3(30%)보다 높은 결과만 표시
                    label = f"{base_trt_model.names[cls]} {conf:.2f}"
                    cv2.rectangle(annotated_frame, (x1, y1), (x2, y2), (255, 0, 0), 2)
                    cv2.putText(annotated_frame, label, (x1, y1-10),
                              cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 0, 0), 2)

        # 커스텀 YOLO 결과 그리기 (빨간색)
        if custom_results[0].boxes is not None:
            for box in custom_results[0].boxes:
                x1, y1, x2, y2 = map(int, box.xyxy[0])
                conf = float(box.conf[0])
                cls = int(box.cls[0])

                if conf > 0.3:
                    label = f"{custom_trt_model.names[cls]} {conf:.2f}"
                    cv2.rectangle(annotated_frame, (x1, y1), (x2, y2), (0, 0, 255), 2)
                    cv2.putText(annotated_frame, label, (x1, y1-10),
                              cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)

        # TensorRT 정보 표시
        fps_text = f"TensorRT: {1/inference_time:.1f} FPS"
        cv2.putText(annotated_frame, fps_text, (10, 30),
                   cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)
        # 방금 프레임에 걸린 추론 속도(FPS)를 측정해서, 프레임 위 왼쪽 상단(10, 30 좌표)에 초록색(0,255,0) 큰 글씨로 표시.

        out.write(annotated_frame)
        frame_count += 1

        if frame_count % 50 == 0:
            avg_fps = frame_count / total_inference_time
            print(f"   처리 중... {frame_count}/{total_frames} ({frame_count/total_frames*100:.1f}%) - 평균 {avg_fps:.1f} FPS")

    cap.release()
    out.release()

    avg_fps = frame_count / total_inference_time
    print(f"✅ TensorRT 결과 영상 저장: {output_path}")
    print(f"📊 평균 처리 속도: {avg_fps:.1f} FPS")

    return avg_fps
```

#### 7️⃣ TensorRT 최적화된 추론 실행
```
print("\n🔥 TensorRT 최적화된 결합 추론 실행...")
tensorrt_fps = tensorrt_combined_inference(video_path, '/content/tensorrt_final_result.mp4')
```

#### 8️⃣ 기존 PyTorch 추론도 실행 (비교용)
```
print("\n🐍 PyTorch 기존 추론 (비교용)...")
def pytorch_combined_inference(video_path, output_path='/content/pytorch_result.mp4'):
    cap = cv2.VideoCapture(video_path) #동영상 파일을 한장씩 읽어들일 준비
    fps = int(cap.get(cv2.CAP_PROP_FPS))
    width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)) # 원본 영상의 프레임 속도, 크기를 읽어와서 아래서 새 영상 만들때 사용

    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    out = cv2.VideoWriter(output_path, fourcc, fps, (width, height)) #결과가 될 새 영상을 저장할 준비

    frame_count = 0
    start_time = time.time() #몇번째 프레임까지 처리했는지, 언제부터 작업을 시작했는지 체크

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        base_results = base_model(frame, verbose=False)
        custom_results = custom_model(frame, verbose=False)
        # 기본 PyTorch YOLO 모델(base_model), 커스텀 PyTorch YOLO 모델(custom_model)을 각각 동일한 프레임에 대해 객체 인식을 실행합니다.

        # 간단한 시각화 (속도 비교용)
        annotated_frame = frame.copy()
        cv2.putText(annotated_frame, "PyTorch", (10, 30),
                   cv2.FONT_HERSHEY_SIMPLEX, 1, (255, 255, 0), 2)

        out.write(annotated_frame)
        frame_count += 1

        if frame_count >= 100:  # 100프레임만 처리 (비교용)
            break

    cap.release()
    out.release()

    total_time = time.time() - start_time
    pytorch_fps = frame_count / total_time
    return pytorch_fps
    # 영상/결과 파일을 모두 닫아 자원을 해제. 전체 걸린 시간을 재서 즉, 초당 몇 프레임까지 PyTorch가 추론 가능한지 계산

pytorch_fps = pytorch_combined_inference(video_path)
#실제로 위 함수를 실행하고, 계산된 PyTorch FPS 값을 변수로 저장.
#이 값이 나중에 TensorRT 결과(FPS)와 비교 기준이 됩니다.
```


#### 9️⃣ 성능 평가 (커스텀 모델)
```
print("\n📊 커스텀 모델 성능 평가:")
metrics = custom_model.val(data='/content/dataset.yaml')  # custom_trt_model 대신 custom_model
print(f"mAP50: {metrics.box.map50:.4f}")
```

#### 🔟 최종 결과 및 비교
```
print("\n" + "="*60)
print("🎯 최종 성능 비교 결과:")
print(f"🐍 PyTorch: {pytorch_fps:.1f} FPS")
print(f"⚡ TensorRT: {tensorrt_fps:.1f} FPS")
print(f"🚀 전체 속도 향상: {tensorrt_fps/pytorch_fps:.2f}x")

print(f"\n📊 모델 정확도 (mAP50): {metrics.box.map50:.4f}")

print("\n🎬 최종 TensorRT 결과 영상:")
Video('/content/tensorrt_final_result.mp4', width=800)

print("\n🎉 TensorRT 최적화 완료!")
print("🔵 파란색 박스: 기본 YOLO 객체들 (TensorRT 최적화)")
print("🔴 빨간색 박스: 커스텀 객체들 (TensorRT 최적화)")
print("💚 초록색 텍스트: 실시간 FPS 표시")

print("\n💾 생성된 파일들:")
print("- tensorrt_final_result.mp4: TensorRT 최적화된 최종 결과")
print("- pytorch_result.mp4: PyTorch 비교용 결과")
print("- yolo11n.engine: 기본 모델 TensorRT 엔진")
print("- best.engine: 커스텀 모델 TensorRT 엔진")
```
---

### 최종 영상 캡쳐본
<img width="1710" height="1008" alt="스크린샷 2025-07-28 16 30 10" src="https://github.com/user-attachments/assets/c04726cb-1254-4246-b7b0-6878e71caa5d" />

### 주의사항
- '/content/dataset.yaml', '/content/dataset/best.pt'와 같은 **경로** 틀리지 않게 **주의하기**
- CPU는 돌아가지 않으므로 **GPU로 런타임 유형 변경** 후 실행하기






