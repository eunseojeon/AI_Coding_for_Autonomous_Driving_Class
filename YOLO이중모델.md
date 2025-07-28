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

