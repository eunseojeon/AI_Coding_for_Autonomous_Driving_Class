# Roboflow 사용법: 
## 로그인부터 API 코드 받기까지

### 1. 회원가입 및 로그인

- **Roboflow 공식 사이트**([roboflow.com](https://app.roboflow.com))에 접속.
- 우측 상단의 **Sign in** 버튼 클릭.
- 구글 계정, 깃허브 계정 등 다양한 방법으로 가입 및 로그인 가능

<img width="1254" height="857" alt="스크린샷 2025-07-24 09 39 11" src="https://github.com/user-attachments/assets/9b4d18a7-512e-4bfb-b5bf-4dce2d32569f" />

- 회원가입 후 워크스페이스(Workspace) 생성 화면이 나오며, 용도(Work, Academia, Hobbies 등)와 워크스페이스 이름을 입력.
- 요금제 선택에서 무료(Community/Public Plan)도 제공됨. 3인 팀, 공개 데이터 등 제약이 있음

### 2. 프로젝트 생성

- 로그인 후 **Create New Project** 버튼 클릭.

<img width="1254" height="857" alt="스크린샷 2025-07-24 09 41 22" src="https://github.com/user-attachments/assets/3583c2e8-57b5-4058-81b0-df1162a034f4" />

- 프로젝트 이름, 타입(Object Detection, Classification 등), 어노테이션 그룹(인식할 객체명) 설정
- 프로젝트 생성 후, 데이터셋 작업을 위한 여러 기능 제공.

### 3. 이미지 업로드 및 라벨링

<img width="1705" height="947" alt="스크린샷 2025-07-25 00 30 18" src="https://github.com/user-attachments/assets/259e3f18-b151-4e8c-9d01-1335aee7768c" />


- **Upload** 메뉴에서 데이터를 업로드(드래그&드롭, 폴더 선택 등)
- **몇 초마다 프레임**을 설정할 건지 정하기 (나같은 경우, 26분짜리 영상을 2초마다 설정하니 500장 정도의 사진이 나옴)
- 업로드된 이미지에 대해 **Save and Continue** 클릭.
- 이후 **Start Annotating** 버튼 클릭 후, 원하는 객체를 박스 등으로 라벨링(어노테이션) 작업 수행

#### ✏️ 라벨링 하는 법
1. **Annotate** 들어간 뒤 제일 처음 사진 클릭하기

<img width="1254" height="857" alt="스크린샷 2025-07-24 09 49 49" src="https://github.com/user-attachments/assets/6e147acb-004b-4739-84b8-7760827c9684" />

2. 객체마다 Classes 지정해주기 (ex. traffic_sign, speed, lane, guardrail 등)
3. 남아있는 사진 전부 다 지정해주면 됨

### 4. 데이터셋 분할 및 버전 생성

- 모든 이미지의 라벨링이 완료되면, **Add images to dataset** 메뉴에서 이미지들을 train/valid/test로 분할.
- **Generate New Version**을 클릭해 새로운 데이터셋 버전 생성
- 필요한 경우 전처리(Augmentation) 옵션을 설정할 수 있음

### 5. 데이터셋 다운로드/API 코드 복사

<img width="1705" height="975" alt="스크린샷 2025-07-25 01 07 37" src="https://github.com/user-attachments/assets/49bb28dc-95df-4a8a-8a06-ffbe237d7387" />

- 데이터셋 버전이 생성되면 versions 들어가서 **Download dataset** 클릭
- 원하는 다운로드 포맷 설정 -> YOLOv8 추천!
  - **Download zip to computer**: 로컬로 직접 다운로드.
  - **Show download code**: 파이썬 등 코드로 바로 다운로드 받을 수 있음
 
<img width="1705" height="975" alt="스크린샷 2025-07-25 01 08 49" src="https://github.com/user-attachments/assets/6bbe91fb-7fc7-4966-a5c3-596ba12b6e07" />

  - Show download code를 누르면 아래처럼 예시 코드가 뜸

#### 예시 (Python 다운로드 코드)
```python
# 예시: roboflow 패키지 설치 및 데이터셋 다운로드
from roboflow import Roboflow

rf = Roboflow(api_key="여기에_API_키를_입력")
project = rf.workspace().project("프로젝트명")
dataset = project.version(1).download("yolov8")
```

### 6. API 키 발급 및 복사

- **좌측 메뉴의 Settings > API Keys**에서 발급 가능
  - 여기서 **Generate new key** 클릭해 새 키 발급.
  - 이미 발급된 키가 있다면 **Copy** 버튼으로 복사.
- API 키는 워크스페이스 단위로 관리되며, 해당 키로 프로젝트의 데이터 및 모델 접근이 가능함.
- 키는 개인 정보이므로 외부에 노출되지 않게 주의



