# 🚗 AI-ccident: 고속도로 CCTV 영상 내 자동차 사고 상황 감지 시스템

**`Capstone Design Project 25-1 | 12조`**

**Team Members:** 18011791 정찬우, 19011727 정준용, 20011290 전우재, 20011901 안도현

---

## 📜 1. 프로젝트 개요 (Overview)

**AI-ccident**는 고속도로에서 발생하는 2차 교통사고의 위험성을 줄이기 위해 개발된 인공지능 기반 사고 감지 시스템입니다. 고속도로 CCTV 영상을 실시간으로 분석하여 **정차 또는 저속 주행하는 차량을 신속하게 탐지**하고, 이를 통해 사고 상황을 조기에 인지하여 후속 차량에 경고하는 것을 목표로 합니다.

본 프로젝트는 **현대자동차 기업 과제**의 일환으로 진행되었으며, 데이터는 비밀유지서약(NDA)에 따라 비공개입니다.

## ✨ 2. 주요 기능 (Features)

* **객체 탐지 (Object Detection):** **YOLOv8** 모델을 사용하여 CCTV 영상 속 차량(자동차, 버스, 트럭)을 탐지합니다.
* **객체 추적 (Object Tracking):** **ByteTrack** 알고리즘을 적용하여 탐지된 각 차량에 고유 ID를 부여하고 프레임 간 이동 경로를 추적합니다.
* **정차 차량 판단:** 추적된 차량의 위치 정보를 기반으로 특정 시간 이상 움직임이 없는 차량을 '정차 차량'으로 판단하는 로직을 구현했습니다.
* **웹 UI 제공:** **Gradio**를 활용하여 사용자가 직접 동영상 파일을 업로드하고 분석 결과를 확인할 수 있는 간단한 웹 인터페이스를 제공합니다.

## ⚙️ 3. 시스템 아키텍처 (System Architecture)



| 단계 | 기술 | 설명 |
| :--- | :--- | :--- |
| **1. 영상 입력** | OpenCV | CCTV 스트리밍 또는 동영상 파일을 입력받습니다. |
| **2. 차량 탐지** | YOLOv8 | 프레임 내 차량 객체를 탐지하고 Bounding Box를 생성합니다. |
| **3. 차량 추적** | ByteTrack | 각 차량의 고유 ID를 할당하고 프레임 간 동선을 추적합니다. |
| **4. 상태 분석** | Custom Logic | 차량의 속도와 위치 변화를 분석하여 정차 여부를 판단합니다. |
| **5. 결과 시각화** | Gradio | 분석 결과를 영상과 텍스트로 사용자에게 보여줍니다. |

## 🛠️ 4. 기술 스택 (Tech Stack)

* **Language:** `Python`
* **Core Libraries:** `PyTorch`, `Ultralytics`, `OpenCV`
* **Object Tracking:** `ByteTrack`
* **UI/Demo:** `Gradio`

## 🚀 5. 실행 방법 (Getting Started)

### 1. 사전 준비 (Prerequisites)

* Python 3.8 이상
* 학습된 모델 가중치 파일: `yolo_tmp_vehicle_detection.pt` (파일명은 예시) 파일을 `weights/` 디렉토리에 위치시켜 주세요.

### 2. 설치 (Installation)

1.  **리포지토리 클론:**
    ```bash
    git clone [https://github.com/your-username/AI-ccident.git](https://github.com/your-username/AI-ccident.git)
    cd AI-ccident
    ```

2.  **필요 라이브러리 설치:**
    ```bash
    pip install -r requirements.txt
    ```
    > **Note:** `requirements.txt` 파일은 아래 명령어로 생성할 수 있습니다.
    > `pip freeze > requirements.txt`

### 3. 실행 (Usage)

프로젝트 루트 디렉토리에서 아래 명령어를 통해 Jupyter Notebook을 실행합니다.
```bash
jupyter notebook ./code/YOLOv8+ByteTrack_Fianl_Ver.ipynb
```
Notebook 실행 후, Gradio UI가 생성되면 영상을 업로드하여 결과를 확인할 수 있습니다.

## ⚠️ 6. 데이터셋 (Dataset)

본 프로젝트에서 사용된 학습 및 평가 데이터는 **현대자동차와의 비밀유지서약(NDA)에 따라 공개할 수 없습니다.** 데이터는 국내 고속도로 CCTV 영상 환경의 특성을 반영하여 구축되었습니다.

## 💡 7. 프로젝트 고찰 (Challenges & Learnings)

* **파라미터 튜닝:** 실제 고속도로 환경의 다양한 변수(차량 속도, 조명 등)에 강건하게 대응하기 위해 ByteTrack의 파라미터를 미세 조정하는 과정을 거쳤습니다.
* **야간 환경 대응:** 야간 영상에서는 차량 형태보다 라이트가 두드러지는 점을 고려하여, 야간 데이터에 특화된 모델 파인튜닝의 필요성을 확인했습니다.
* **실시간 처리 성능:** T4 GPU 환경에서 약 **25.4 FPS**의 처리 속도를 확보하여 준수한 실시간 탐지 성능을 검증했습니다. (PPT 내용 기반)

## 🎯 8. 향후 계획 (Future Work)

* 실제 CCTV 스트리밍 연동을 통한 실시간 사고 감지 시스템 구축
* 모델 경량화를 통한 엣지 디바이스(Edge Device) 적용 가능성 탐색
* 다양한 돌발 상황(낙하물, 역주행 등) 탐지 기능 추가