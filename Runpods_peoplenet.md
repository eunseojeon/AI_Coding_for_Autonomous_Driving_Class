# 📝 Runpods에서 peoplenet 정리하기

### 교수님께서 주신 코드 정리하기
[코드 정리](0730_peoplenet.ipynb)
```
#Open In Colab

!apt update && apt install -y unzip wget ffmpeg
```
- 의미: 필요한 도구들을 한번에 설치하는 단계.
- `unzip`: 압축파일 풀기용.
- `wget`: 인터넷에서 파일 다운로드용.
- `ffmpeg`: 동영상·음성 변환 도구.
- 실행 위치: Colab/Runpod의 첫 코드셀에서 반드시 맨처음 입력.

```
# JupyterLab 셀에서 이렇게 실행하세요 (! 붙이기)

# 현재 작업 폴더(경로) 확인
!pwd  #보통 /workspace처럼 출력

# /workspace 폴더에서 파일 이름에 ngc가 포함된 파일 찾기(검색)
!find /workspace -name "*ngc*" -type f

# NVIDIA NGC CLI 설치 파일 다운로드(조용히 다운로드)
!wget -q https://ngc.nvidia.com/downloads/ngccli_reg_linux.zip

# 위에서 다운받은 ngccli_reg_linux.zip 파일을 풀기
!unzip -o ngccli_reg_linux.zip

# NGC CLI 파일에 실행 권한 주기
!chmod +x ngc-cli/ngc
```
```
# 설정 실행
!./ngc-cli/ngc config set
```
- NGC CLI 기본 설정(인증 정보 입력)이 필요하면 입력.
- 프롬프트 나오면 키(토큰) 같은 것 입력. 보통 GPU 관련 계정 필요.

     
```
# PeopleNet 관련 폴더/파일이 있는지 확인(없으면 다음 단계로 넘어감)
!ls -la /workspace/peoplenet*
     

# NVIDIA에서 PeopleNet 모델파일 다운로드(자동으로 받아옴)
!./ngc-cli/ngc registry model download-version nvidia/tao/peoplenet:pruned_quantized_decrypted_v2.3.4
     
# 다운로드받은 모델 파일이 왔는지 재확인
!ls -la peoplenet*
```
```
# 실제로 필요한 패키지만 설치
!pip install onnxruntime yt-dlp opencv-python numpy
```
- `onnxruntime`: ONNX 모델 추론.
- `yt-dlp`: 유튜브 영상 다운로드.
- `opencv-python`: 이미지 처리.
- `numpy`: 수치 연산용.
- 필요한 Python 라이브러리 한줄로 설치.



### 완전한 디버깅 NVIDIA PeopleNet 코드 (초보자용)
```
import cv2
import numpy as np
import subprocess
import os
import json
import time
import onnxruntime as ort
```
- 외부 라이브러리 임포트(불러오기): cv2(OpenCV), numpy, subprocess(시스템 명령), os(파일), onnxruntime, 등
→ 영상처리, 수치연산, 시스템 명령, ONNX 추론 처리용입니다.

```
class DebugNVIDIAPeopleNet: # NVIDIA PeopleNet 검출/디버깅+영상 분석까지 모두 포함한 클래스

    def __init__(self):
        """
        디버깅 NVIDIA PeopleNet
        """
        print("🚀 디버깅 NVIDIA PeopleNet 시작...")

        # 모델 경로 설정
        self.model_path = "/workspace/peoplenet_vpruned_quantized_decrypted_v2.3.4/resnet34_peoplenet_int8.onnx"
        self.classes = ['person', 'bag', 'face']
        self.colors = [(0, 255, 0), (255, 0, 0), (0, 0, 255)]  # 녹색, 파랑, 빨강

        # 모델 로드
        self.setup_model()
```
1) `__init__(self)`
- 객체 생성 시 자동 호출.
- 모델 경로/클래스 정의(사람, 가방, 얼굴 지정), 색상 지정.
- 바로 setup_model() 실행(모델 준비 및 테스트).

---

```
    def setup_model(self):
        """
        모델 설정
        """
        try:
            print(f"📁 모델 경로 확인: {self.model_path}")

            # 파일 존재 확인
            if not os.path.exists(self.model_path):
                print("❌ 모델 파일이 없습니다. 모델을 찾는 중...")
                self.find_model()
                return

            # ONNX Runtime 설정
            providers = ['CPUExecutionProvider']
            self.session = ort.InferenceSession(self.model_path, providers=providers)

            # 입출력 정보
            input_info = self.session.get_inputs()[0]
            output_info = self.session.get_outputs()

            self.input_name = input_info.name
            self.output_names = [output.name for output in output_info]

            print(f"✅ 모델 로드 성공!")
            print(f"📊 입력: {input_info.name}, 형태: {input_info.shape}")
            print(f"📊 출력 개수: {len(output_info)}")

            self.model_loaded = True

            # 테스트 추론
            self.test_model()

        except Exception as e:
            print(f"❌ 모델 로드 실패: {e}")
            self.model_loaded = False
```
2) `setup_model(self)`
- 모델 경로에 파일이 있는지 확인.
- 없으면 find_model()로 디렉토리 내에서 자동 탐색.
- 있으면 ONNX 모델을 로드하고, 입력/출력 정보 파악.
- 모델 정상동작 확인 위해 test_model() 호출.

---

```
    def find_model(self):
        """
        모델 파일 찾기
        """
        import glob

        search_patterns = [
            "/workspace/peoplenet*/resnet34_peoplenet_int8.onnx",
            "/workspace/*/resnet34_peoplenet_int8.onnx"
        ]

        for pattern in search_patterns:
            files = glob.glob(pattern)
            if files:
                self.model_path = files[0]
                print(f"✅ 모델 발견: {self.model_path}")
                self.setup_model()
                return

        print("❌ NVIDIA PeopleNet 모델을 찾을 수 없습니다")
        print("💡 NGC에서 모델을 다운로드하세요:")
        print("   !./ngc-cli/ngc registry model download-version nvidia/tao/peoplenet:pruned_quantized_decrypted_v2.3.4")
        self.model_loaded = False
```
3) `find_model(self)`
- 지정 경로 또는 유사 경로에서 모델 파일 자동 탐색.
- 찾으면 바로 위 setup_model() 재실행.

---

```
    def test_model(self):
        """
        모델 테스트
        """
        try:
            print("🧪 모델 테스트 중...")

            # 더미 입력
            dummy_input = np.random.randn(1, 3, 544, 960).astype(np.float32)
            outputs = self.session.run(self.output_names, {self.input_name: dummy_input})

            print(f"✅ 테스트 성공!")
            for i, output in enumerate(outputs):
                print(f"   출력 {i}: {output.shape}, 범위 [{output.min():.3f}, {output.max():.3f}]")

        except Exception as e:
            print(f"❌ 테스트 실패: {e}")
```
4) `test_model(self) (실행 즉시 자동)`
- 더미 데이터를 이용해 모델이 제대로 작동하는지 확인
→ 추론 출력 shape, 값 분포, 최소/최대값 등 상세 로그 출력

---

```
    def preprocess_frame(self, frame):
        """
        프레임 전처리
        """
        # 960x544로 리사이즈
        resized = cv2.resize(frame, (960, 544))

        # BGR → RGB
        rgb_frame = cv2.cvtColor(resized, cv2.COLOR_BGR2RGB)

        # 정규화
        normalized = rgb_frame.astype(np.float32) / 255.0

        # HWC → CHW
        chw_frame = np.transpose(normalized, (2, 0, 1))

        # 배치 차원 추가
        batch_frame = np.expand_dims(chw_frame, axis=0)

        return batch_frame
```
5) `preprocess_frame(self, frame)`
- 단일 프레임(이미지)을 모델 입력 형식(960x544, RGB, 정규화, shape 변환)으로 변환.

---

```
    def detect_people(self, frame, debug=True):
        """
        사람 검출 (디버깅 모드)
        """
        if not self.model_loaded:
            if debug:
                print("❌ 모델이 로드되지 않음")
            return []

        try:
            if debug:
                print(f"🔍 입력 프레임: {frame.shape}")

            # 전처리
            input_data = self.preprocess_frame(frame)
            if debug:
                print(f"📊 전처리 완료: {input_data.shape}")

            # 추론
            outputs = self.session.run(self.output_names, {self.input_name: input_data})
            if debug:
                print(f"🤖 추론 완료")
                for i, output in enumerate(outputs):
                    print(f"   출력 {i}: {output.shape}")
                    print(f"   범위: [{output.min():.4f}, {output.max():.4f}]")
                    print(f"   평균: {output.mean():.4f}")

                    # 높은 값 개수 확인
                    high_01 = len(output[output > 0.1])
                    high_03 = len(output[output > 0.3])
                    high_05 = len(output[output > 0.5])
                    print(f"   값 분포: >0.1({high_01}), >0.3({high_03}), >0.5({high_05})")

            # 후처리
            detections = self.postprocess_debug(outputs, frame.shape, debug=debug)

            if debug:
                print(f"🎯 최종 검출: {len(detections)}개")
                for det in detections:
                    print(f"   - {det['class']}: {det['confidence']:.3f}")

            return detections

        except Exception as e:
            if debug:
                print(f"❌ 검출 오류: {e}")
            return []
```
6) `detect_people(self, frame, debug=True)`
- 실제 이미지에서 사람/가방/얼굴 검출.
- 입력 프레임 전처리 → 모델 추론 → 검출 결과(바운딩박스 등) 후처리
- 중간 단계별로 풍부한 디버그(로그) 정보 출력해 초보자도 쉽게 문제 파악.

---

```
    def postprocess_debug(self, outputs, original_shape, debug=True):
        """
        디버깅 후처리
        """
        detections = []

        try:
            predictions = outputs[0]  # 첫 번째 출력
            orig_h, orig_w = original_shape[:2]

            if debug:
                print(f"🔍 후처리 시작: {predictions.shape}")

            # 배치 차원 제거
            if len(predictions.shape) == 4:
                predictions = predictions[0]  # (3, 34, 60)

            num_classes, grid_h, grid_w = predictions.shape

            if debug:
                print(f"📊 그리드: {num_classes} 클래스, {grid_h}x{grid_w}")

            # 각 클래스별 처리
            for class_idx in range(min(num_classes, len(self.classes))):
                class_name = self.classes[class_idx]
                class_pred = predictions[class_idx]  # (34, 60)

                # 최고값 찾기
                max_val = float(class_pred.max())
                if debug:
                    print(f"   {class_name} 최대값: {max_val:.4f}")

                # 다양한 임계값으로 테스트
                thresholds = [0.05, 0.1, 0.2, 0.3, 0.4]

                for threshold in thresholds:
                    high_positions = np.where(class_pred > threshold)

                    if len(high_positions[0]) > 0:
                        if debug:
                            print(f"   {class_name} 임계값 {threshold}: {len(high_positions[0])}개 후보")

                        # 상위 5개만 처리
                        for i in range(min(5, len(high_positions[0]))):
                            y_idx = high_positions[0][i]
                            x_idx = high_positions[1][i]
                            confidence = float(class_pred[y_idx, x_idx])

                            # 좌표 변환
                            center_x = (x_idx + 0.5) / grid_w
                            center_y = (y_idx + 0.5) / grid_h

                            # 바운딩 박스 크기
                            if class_name == 'person':
                                box_w, box_h = 0.12, 0.20
                            elif class_name == 'bag':
                                box_w, box_h = 0.06, 0.08
                            else:  # face
                                box_w, box_h = 0.04, 0.05

                            # 이미지 좌표로 변환
                            x1 = int((center_x - box_w/2) * orig_w)
                            y1 = int((center_y - box_h/2) * orig_h)
                            x2 = int((center_x + box_w/2) * orig_w)
                            y2 = int((center_y + box_h/2) * orig_h)

                            # 경계 체크
                            x1 = max(0, min(x1, orig_w-1))
                            y1 = max(0, min(y1, orig_h-1))
                            x2 = max(x1+10, min(x2, orig_w))
                            y2 = max(y1+10, min(y2, orig_h))

                            detections.append({
                                'bbox': [x1, y1, x2, y2],
                                'confidence': confidence,
                                'class_id': class_idx,
                                'class': class_name,
                                'method': f'Debug_T{threshold}'
                            })

                        break  # 첫 번째 성공하는 임계값에서 중단

            # 중복 제거 (간단버전)
            if detections:
                detections = self.simple_nms(detections)

        except Exception as e:
            if debug:
                print(f"❌ 후처리 오류: {e}")

        return detections
```
7) `postprocess_debug(self, outputs, original_shape, debug=True)`
- 모델 원시 출력을 실제 객체 검출(box, 신뢰도 등)로 변환
- 클래스별 임계값 여러개 돌리며 바운딩박스, 신뢰도 추출.
- 중복(겹침) 제거 위해 simple_nms() 호출

---

```
    def simple_nms(self, detections, iou_threshold=0.5):
        """
        간단한 NMS
        """
        if not detections:
            return detections

        # 신뢰도 순 정렬
        detections = sorted(detections, key=lambda x: x['confidence'], reverse=True)

        final_detections = []

        for current in detections:
            should_keep = True

            for kept in final_detections:
                if current['class'] == kept['class']:
                    iou = self.calculate_iou(current['bbox'], kept['bbox'])
                    if iou > iou_threshold:
                        should_keep = False
                        break

            if should_keep:
                final_detections.append(current)

        return final_detections
```
8) `simple_nms(self, detections, iou_threshold=0.5)`
- NMS(Non-Maximum Suppression), 즉 한 객체 당 겹친 검출 제거
- 가장 확신 높은 것만 남김

---

```
    def calculate_iou(self, box1, box2):
        """
        IoU 계산
        """
        x1_1, y1_1, x2_1, y2_1 = box1
        x1_2, y1_2, x2_2, y2_2 = box2

        # 교집합
        x1_i = max(x1_1, x1_2)
        y1_i = max(y1_1, y1_2)
        x2_i = min(x2_1, x2_2)
        y2_i = min(y2_1, y2_2)

        if x2_i <= x1_i or y2_i <= y1_i:
            return 0.0

        intersection = (x2_i - x1_i) * (y2_i - y1_i)

        # 합집합
        area1 = (x2_1 - x1_1) * (y2_1 - y1_1)
        area2 = (x2_2 - x1_2) * (y2_2 - y1_2)
        union = area1 + area2 - intersection

        return intersection / union if union > 0 else 0.0
```
9) `calculate_iou(self, box1, box2)`
- 두 바운딩 박스의 중첩률(IoU) 계산해서 중복 판단 근거 제공

---

```
    def draw_detections(self, frame, detections):
        """
        검출 결과 그리기
        """
        for detection in detections:
            x1, y1, x2, y2 = detection['bbox']
            confidence = detection['confidence']
            class_name = detection['class']
            class_id = detection['class_id']
            method = detection.get('method', 'Unknown')

            # 색상
            color = self.colors[class_id]

            # 바운딩 박스
            cv2.rectangle(frame, (x1, y1), (x2, y2), color, 2)

            # 라벨
            label = f"{class_name}: {confidence:.2f} ({method})"
            cv2.putText(frame, label, (x1, y1-10),
                       cv2.FONT_HERSHEY_SIMPLEX, 0.5, color, 1)

        return frame
```
10) `draw_detections(self, frame, detections)`
- OpenCV로 이미지 위에 바운딩박스와 라벨 표시(클래스명, 신뢰도, 방법)

---

```
    def download_youtube_video(self, url):
        """
        YouTube 다운로드
        """
        try:
            print(f"📺 YouTube 다운로드: {url}")
            output_path = "/workspace/debug_input_video.mp4"

            cmd = ["yt-dlp", "--format", "best[height<=720]", "--output", output_path, url]
            result = subprocess.run(cmd, capture_output=True, text=True, check=True)

            if os.path.exists(output_path):
                print(f"✅ 다운로드 완료: {output_path}")
                return output_path
            return None

        except Exception as e:
            print(f"❌ 다운로드 실패: {e}")
            return None
```
11) `download_youtube_video(self, url)`
- 지정 유튜브 주소에서 영상을 다운로드해서 저장
- 파일 다운로드 성공 여부까지 체크하여 반환


---

```
    def test_video_frames(self, video_path, num_frames=5):
        """
        비디오 프레임 테스트
        """
        if not os.path.exists(video_path):
            print(f"❌ 비디오 파일이 없습니다: {video_path}")
            return

        cap = cv2.VideoCapture(video_path)
        if not cap.isOpened():
            print("❌ 비디오를 열 수 없습니다")
            return

        print(f"🎬 {num_frames}개 프레임 테스트 시작...")

        frame_interval = 100  # 100프레임마다 테스트

        for i in range(num_frames):
            # 프레임 이동
            cap.set(cv2.CAP_PROP_POS_FRAMES, i * frame_interval)

            ret, frame = cap.read()
            if not ret:
                break

            print(f"\n🎯 프레임 {i * frame_interval} 테스트:")
            detections = self.detect_people(frame, debug=True)

            if detections:
                print(f"✅ {len(detections)}개 검출 성공!")
                for det in detections:
                    print(f"   - {det['class']}: {det['confidence']:.3f}")
            else:
                print("❌ 검출 없음")

        cap.release()
        print("\n🏁 프레임 테스트 완료")
```
12) `test_video_frames(self, video_path, num_frames=5)`
- 동영상을 일정 간격마다 여러 프레임(예시 5개) 뽑기
- 각 프레임마다 검출(사람, 가방, 얼굴) 실행 → 결과, 로깅

---

```
# 메인 실행 함수
def run_debug_peoplenet():
    """
    디버깅 PeopleNet 실행
    """
    print("🔧 디버깅 NVIDIA PeopleNet 시작")
    print("=" * 60)

    # 분석기 생성
    analyzer = DebugNVIDIAPeopleNet()

    if not analyzer.model_loaded:
        print("❌ 모델을 로드할 수 없습니다. NGC에서 모델을 다운로드하세요.")
        return None

    # YouTube 동영상 다운로드
    youtube_url = "https://www.youtube.com/watch?v=SzRzYvQq0aQ"
    video_path = analyzer.download_youtube_video(youtube_url)

    if not video_path:
        print("❌ 동영상 다운로드 실패")
        return None

    # 프레임 테스트
    analyzer.test_video_frames(video_path, num_frames=5)

    return analyzer
```
13) `def run_debug_peoplenet():` 실행 메인 함수
1. DebugNVIDIAPeopleNet 인스턴스 생성(모델 탐색, 로딩 및 테스트 자동진행)
2. 유튜브(예제) 영상 다운로드
3. 5개 프레임에서 검출 실험/디버깅 실행
4. 실패하면 어디서 막혔는지 단계별로 중단 및 원인 메시지 출력.


---

```
# 실행
print("🚀 완전한 디버깅 PeopleNet 코드")
print("🔍 상세한 로그와 함께 실행됩니다")
result = run_debug_peoplenet()
```
- 설명 문구를 보여주고, 바로 전체 통합 실행
- 중간 과정 모두 로그로 보여줌(에러 위치, 검출 결과 등)
- 코드의 진입점(맨 마지막까지 그대로 실행되게 되어 있음)
