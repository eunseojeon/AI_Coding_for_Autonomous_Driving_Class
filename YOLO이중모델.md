# 📝 YOLO 이중모델 영상 객체 검출 및 시각화 (07/28)
- [교수님께서 주신 코드](0728_YOLO_이중모델_영상_객체_검출_및_시각화.ipynb)

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
```
- 위 코드는 아래 코드들에서 오류났을 때, 새로운 파일이 생기니까 그 파일을 다 삭제해주는 코드! (리셋느낌)
---

### ✏️ Google Colab에서 사용자의 Google Drive를 Colab 환경에 연결하기
```
from google.colab import drive
drive.mount('/content/drive')
```
--- 

### ❗️ZIP 파일을 코랩작업 폴더(/content/)로 복사
```!cp "/content/drive/MyDrive/dataset.zip" "/content/"```

### ❗️복사한 ZIP 파일(dataset.zip)의 압축을 풀어서 /content/ 폴더에 저장
```!unzip -o /content/dataset.zip -d /content/```

### ❗️/content/ 폴더에 어떤 파일과 폴더가 있는지 자세히(숨김파일, 권한, 용량 등 포함) 목록을 출력
```!ls -la /content/```

- (현재 코랩 GPU사용이 끝나서 경로를 불러올 수 없음ㅜㅜ 내일 다시 경로 찾아서 캡쳐한 후 보기 편하게 사진으로 정리할 예정)
---

### 1️⃣ ultralytics(= YOLOv8 포함 최신 YOLO 프레임워크) 라이브러리를 설치
```!pip install ultralytics```

### 2️⃣ ultralytics 패키지에서 YOLO 클래스를 불러오고(glob, os는 파일 경로·검색에 사용) 설치가 잘 되었는지 확인 겸 불러옴.
```from ultralytics import YOLO
import glob
import os

print("✅ ultralytics 설치 완료!")
```

### 3️⃣ Colab 내 /content/dataset/best.pt 위치에 있는 이미 학습된 YOLO 모델(가중치)를 불러옵니다.
```model = YOLO('/content/dataset/best.pt')```

### 4️⃣ YouTube 영상 다운로드
```!pip install yt-dlp
!yt-dlp -f 'best[height<=720]' -o '/content/test_video.%(ext)s' 'https://www.youtube.com/watch?v=AxLmroTo3rQ'
```

- yt-dlp 라이브러리를 설치하고 YouTube 영상(지정된 URL)을 720p 이하 화질로 `/content/test_video.확장자` 형태로 다운로드합니다.
- (확장자는 실제 영상에 따라 mp4, webm, avi 등 달라질 수 있어요)


### 5️⃣ 다운로드된 파일 찾기 (확장자가 다를 수 있음)
```
video_files = glob.glob('/content/test_video.*')
if video_files:
    video_path = video_files[0]
    print(f"📹 다운로드된 영상: {video_path}")

    # 찾은 영상 파일을 YOLO 모델에 넣어 객체 탐지 등 추론을 수행.
    results = model(video_path)

    # 결과가 있으면(=추론이 정상 실행되면) 표시 (영상의 경우 첫 번째 프레임만)
    if results:
        results[0].show()
else:
    print("❌ 영상 다운로드 실패")

# 기존 검증 데이터로 성능 측정
print("\n📊 모델 성능 평가:")
metrics = model.val(data='/content/dataset/dataset.yaml') # Use the fixed YAML file
print(f"mAP50: {metrics.box.map50:.4f}")
print(f"mAP50-95: {metrics.box.map:.4f}")
print(f"Precision: {metrics.box.mp:.4f}")
print(f"Recall: {metrics.box.mr:.4f}")

print("\n✅ 모든 작업 완료!")
```

---

### 💻 dataset.yaml 파일 내용 확인
```
print("📋 dataset.yaml 파일 내용:")
with open('/content/dataset/dataset.yaml', 'r') as f:
    yaml_content = f.read()
    print(yaml_content)
```
- 📋 dataset.yaml 파일 내용:
```
path: /content/dataset
train: train/images
val: valid/images
names:
  0: lane
  1: traffic_sign
```

---
```
!pip install ultralytics yt-dlp

from ultralytics import YOLO
import glob
```
- `ultralytics`: 최신 YOLO 모델 사용을 위한 라이브러리. YOLOv8 등 최신 버전을 쉽게 사용할 수 있게 해줌
- `yt-dlp`: 유튜브나 다른 동영상 플랫폼에서 영상을 다운로드하는 도구. youtube-dl의 fork 버전

### yaml 수정 (핵심 문제 해결)
```
yaml_fix = '''path: /content/dataset
train: train/images
val: /content/dataset/valid/images
names:
  0: lane
  1: traffic_sign
nc: 2'''

with open('/content/dataset/dataset_fixed.yaml', 'w') as f:
    f.write(yaml_fix)
```
- 용도: 데이터셋 설정용 yaml 파일 내용 정의 후 저장
- path: 데이터 루트 경로 (예: `/content/dataset`)
- train: 훈련 이미지 폴더 경로(루트 기준 상대 경로)/content/dataset/train/images 로 해석됨
- val: 검증(validation) 이미지 폴더 경로 (절대경로로 지정됨)
- names: 클래스 이름 매핑 (0번 클래스='lane', 1번 클래스='traffic_sign')
- nc: 클래스 개수 (2개)
- 이 yaml 파일은 모델 학습과 검증 시 클래스 및 데이터 경로를 읽는 데 사용됨.
- 원본 yaml파일 경로나 경로 설정 잘못된 경우 이를 해결하기 위한 수정판을 직접 씀.

<img width="475" height="200" alt="스크린샷 2025-07-28 19 24 36" src="https://github.com/user-attachments/assets/2aad478d-3083-496f-af60-b15bc772fa9c" />

- 내가 가지고 있는 yaml파일도 똑같이 수정해줌

### 모델 로드 & 영상 다운로드
```
model = YOLO('/content/dataset/best.pt') # 인자로 사전학습/커스텀 학습된 모델 가중치 (best.pt) 파일 경로 지정
!yt-dlp -f 'best[height<=720]' -o '/content/test_video.%(ext)s' 'https://www.youtube.com/watch?v=AxLmroTo3rQ' #유튜브 영상 다운로드
```
### 다운로드된 영상 경로 찾기 및 추론
```
video_files = glob.glob('/content/test_video.*')
if video_files:
    video_path = video_files[0]
    print(f"📹 다운로드된 영상: {video_path}")

    # 찾은 영상 파일을 YOLO 모델에 넣어 객체 탐지 등 추론을 수행.
    results = model(video_path)

    # 결과가 있으면(=추론이 정상 실행되면) 표시 (영상의 경우 첫 번째 프레임만)
    if results:
        results[0].show()
else:
    print("❌ 영상 다운로드 실패")

# 성능 평가
print("\n📊 모델 성능 평가:")
metrics = model.val(data='/content/dataset/dataset_fixed.yaml')
print(f"mAP50: {metrics.box.map50:.4f}")
print(f"mAP50-95: {metrics.box.map:.4f}")
print(f"Precision: {metrics.box.mp:.4f}")
print(f"Recall: {metrics.box.mr:.4f}")

print("\n✅ 모든 작업 완료!")
```
- metrics 객체의 box 속성을 통해 박스 단위의 성능 지표 반환
- mAP50: IoU 0.5 기준 평균 정밀도 (mean Average Precision)
- mAP50-95: IoU 0.5 ~ 0.95 까지 단계별 mAP 평균값
- Precision(mp): 정확도 (클래스 예측 중 정답 비율)
- Recall(mr): 재현율 (실제 객체 중 검출된 비율)
- .4f: 소수점 4자리까지 포맷팅 출력







