# 📘 AI 학습 정리

## 1. About GitHub, Markdown, Colab
- [GitHub 사용법](#github-사용법)
- [Markdown 문법](#markdown-문법)  
- [Colab 기초](#colab-기초)

---

## GitHub 사용법

### ✅ GitHub 계정 만드는 순서 (2025년 기준)

1. **웹 브라우저 열기**
   크롬(Chrome), 엣지(Edge), 사파리(Safari) 중 편한 걸 사용하세요.

2. **GitHub 웹사이트 접속**
   주소창에 아래 주소를 입력하고 Enter 누르세요: https://github.com

3. **회원가입 시작하기**
   화면 오른쪽 위 또는 중간에 있는 Sign up 버튼 클릭

4. **이메일 주소 입력**
   평소 자주 사용하는 이메일을 입력

5. **비밀번호 만들기**
   영어 대문자, 소문자, 숫자, 특수문자를 섞어 안전하게!
   예시: Git1234!hub

6. **사용자 이름(Username) 정하기**
   나만의 고유한 이름을 지어요 (다른 사람이 쓰고 있으면 불가)
   - 예시: jetsunmom, sungsookjang66 등
   - 영어, 숫자, 하이픈(-) 가능 (띄어쓰기 ❌)

### ✅ Repository 만들기 순서

1. **GitHub에 로그인 후 New Repository 클릭**
 <img width="1685" alt="스크린샷 2025-07-04 13 08 47" src="https://github.com/user-attachments/assets/2451f10e-e9b4-402e-b4ba-f4e741e53545" />
 
2. **Repository 이름 입력**
3. **Public/Private 선택**
4. **README.md 파일 생성 체크**
5. **Create repository 버튼 클릭**
---

## Markdown 문법

### 🔰 1. 마크다운(Markdown)이란?

Markdown은 글을 **쉽게 꾸미기 위한 문법**입니다. HTML보다 간단하게 **제목, 목록, 굵은 글씨, 링크, 코드블록** 등을 작성 가능합니다.
GitHub에서는 `README.md` 파일을 통해 마크다운을 많이 사용합니다.



### 🛠️ 2. GitHub에서 마크다운 사용하려면?

1. **GitHub 계정**을 만들고
2. 새 **Repository**를 만든 뒤
3. `README.md` 파일을 추가해서
4. 마크다운 문법을 사용하여 내용을 입력하면 됩니다.



### ✍️ 3. 기본 마크다운 문법 정리

| 기능        | 문법               | 예시                         | 결과                       |
| --------- | ---------------- | -------------------------- | ------------------------ |
| 제목(Title) | `#`, `##`, `###` | `## 내 프로젝트`                | 내 프로젝트                   |
| 굵게        | `**굵게**`         | `**중요**`                   | **중요**                   |
| 기울임       | `*기울임*`          | `*강조*`                     | *강조*                     |
| 목록        | `-`, `*`         | `- 사과` <br> `- 배`          | ● 사과 <br> ● 배            |
| 숫자 목록     | `1.`, `2.`       | `1. 첫째` <br> `2. 둘째`       | 1. 첫째 <br> 2. 둘째         |
| 링크        | `[이름](주소)`       | `[구글](https://google.com)` | [구글](https://google.com) |
| 이미지       | `![이름](이미지주소)`   | `![고양이](cat.jpg)`          | ![고양이](img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/6f4a6001-dbc8-4349-8b83-93643e504c5a")          |
| 코드블록      | \`\`\`python     | `print("Hello")`           | 코드박스                     |
| 인라인 코드    | \`코드\`           | \`a = 3\`                  | `a = 3`                  |
| 구분선       | `---`            | `---`                      | ―――                      |


### 📷 4. 사진 넣는 방법
**복사 후 붙여넣기**

![귀여운고양이사진_(1)](https://github.com/user-attachments/assets/df528223-e781-4136-a7d4-5a1bdffcd99c)


### ⛓️‍💥 5. 링크 삽입 방법
- 외부 링크: [구글](https://google.com)
- 내부 파일/폴더 링크: [문서](./docs/README.md)
- 문서 내 특정 위치(헤더)로 이동: [섹션 이동](#섹션-제목)


### 🧱 6. 코드 블록 및 인라인 코드
- 인라인 코드: `코드`
- 여러 줄 코드 블록: <pre> ```python print("Hello, world!") ``` </pre>
- 언어 하이라이팅: 코드 블록 첫 줄에 언어명 입력


---

## 📝Colab 기초  


### ❓1. Colab이란?

- **무료**로 파이썬 코드를 작성하고 실행할 수 있는 웹 기반 도구
- **코드**와 **설명**을 섞어서 사용 가능


### 🏃🏻‍♀️2. Colab 시작 방법

1. **구글에 "Colab" 검색** 후 접속
2. **NEW BOOK 만들기** 클릭
3. **코드 셀** 과 **텍스트 셀** 사용



### 🙋🏻‍♀️3. Colab의 셀(Cell) 종류

| 셀 종류   | 설명                        | 사용 방법 예시                    |
| --------- | --------------------------- | ---------------------------------- |
| 코드 셀   | 파이썬 코드 입력 및 실행    | `print("안녕하세요!")`             |
| 텍스트 셀 | 마크다운으로 설명 작성 가능 | **굵게**, *기울임* 등 마크다운 사용 |



### 📶4. 마크다운 문법 요약 (Colab에서 사용)

| 기능           | 마크다운 문법 예시                | 결과 예시                  |
| -------------- | -------------------------------- | -------------------------- |
| 제목           | `# 제목1``## 제목2`          | # 제목1## 제목2        |
| 굵게           | `**굵게**` 또는 `__굵게__`       | **굵게**                   |
| 기울임         | `*기울임*` 또는 `_기울임_`       | *기울임*                   |
| 순서 없는 목록 | `- 사과``- 바나나`           | - 사과- 바나나         |
| 순서 있는 목록 | `1. 첫 번째``2. 두 번째`     | 1. 첫 번째2. 두 번째   |
| 인용문         | `> 인용문 예시입니다.`           | > 인용문 예시입니다.       |



### ✅5. Colab에서 마크다운 사용 방법

1. **셀 추가** 버튼 클릭
2. **텍스트 셀** 선택
3. 작성 후
4. **실행** 하면 보임


### 👍6. 파이썬 코드 실행 방법

1. **코드 셀** 선택
2. 파이썬 코드 입력 
3. **실행** 하면 결과가 아래에 나옴


### ⚠️7. **중요한 부분**
<img width="955" alt="스크린샷 2025-07-04 13 34 15" src="https://github.com/user-attachments/assets/b8e9a421-e07e-4f7b-baa9-9dcac453e41c" />


---

## 2. About Python3
- [Python basic](./python3.md)
- https://www.w3schools.com/

--- 
## 3.  data structure / data sciencs

- [데이터 구조 개요](./data_structures.md)
- [Pandas](./pandas.md)
- [NumPy](./NumPy.md)
- [Matplotlib](./Matplotlib.md)

## 4. Machine Learning

- [Machine Learning Basic](./ml_basic.md)
- [모델 훈련 및 평가](./ml_test.md)

## 5. OpenCV

- [OpenCV Basic](./OpenCV.md)
- [이미지 처리](./image_test.md)

  
## 6. CNN(Convolution Neural Network
- [CNN_Basic](./CNN.md)
- [CNN_자율주행 관련 코드](./cnn_test.md)

## 7. Ultralytics
- [Ultralytics_YOLOv8_Basic](./YOLOv8.md)
- [Ultralytics_YOLO11_Basic](./YOLO11.md)
- [Ultralytics_YOLO12_Basic](Ultralytics_v12.md)
  
## 8. TensorRT vs PyTorch 
- [PyTorch_Basic](./PyTorch_basic.md)
- [TensorRT](./TensorRT_test.md)
- [YOLOv12](./YOLOv12_test.md)

## 9. TAO Toolkit on RunPod
- [TAO_사용법](.TAO_install.md)
- [TAO_Toolkit](.TAO_Toolkit.md)

## 10. 칼만필터, CARLA, 경로 알고리즘
- [kalman](.kalman.md)
- [CARLA_simulator](.CARLA.md)

## 11. ADAS & (ADAS TensorRT vs PyTorch)
- [adas_basic](.adas_basic.md)
- [TensorRT vs PyTorch 비교](.vs.md)
- 
