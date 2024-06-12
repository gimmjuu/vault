---
tags:
  - DL
---

# DeepLearning

➰　Focusing on model training

### 📌 기본 용어 정리

[ https://wikidocs.net/book/5942 ]

## 🎇 Training Flow

    1. Data preprocessing
    2. Model building
    3. Model compiling
        a. Initialized model (*architecture) save
        b. Set callback function
    4. Model fitting
        a. Trained model (*full model or only weight) save
    5. Model predicting or evaluating

## 🎇 Image Classification Model

### 🔥 ResNet
- 네트워크의 크기를 키워(파라미터를 늘려) 모델 성능을 높인다.

### 🔥 VGG
- 상하좌우 및 중심을 볼 수 있는 가장 작은 컨볼루션 필터(3x3, 1 strides) 만으로 깊은 레이어(16-19 layers)를 만들어 성능을 높인다.

### 🔥 Xception
- 파라미터를 효율적으로 활용하여 성능을 높이기 위해 Depthwise Separable Convolution을 활용한다.

### 🔥 MobileNet
- 효율적인 연산을 위해 Depthwise Separable Convolution을 적절히 활용하면서, 모바일 기기에서 돌아갈 수 있을만큼 경량한 구조를 설계하는데 집중했다.

## 🎇 Object Detection Model
➰　YOLO (You Only Look Once)

### 🔥 YOLO v5
- YOLO 컴퓨터 비전 모델 제품군의 하나
- 객체 감지(Object Detection)에 사용한다.
- YOLO v3 PyTorch의 확장
    - 객체 주위에 상자를 그리고 해당 클래스를 예측한다.

### 📌 Yolo 신경망 주요 3 구조
- `BackBone`: 세분성으로 이미지 특징을 집계하고 형성하는 컨볼루션 신경망
- `Neck`: 이미지 특징을 혼합하고 결합하여 예측에 전달하는 일련의 레이어
- `Head`: 목의 특징을 소비하고 상자 및 클래스 예측 단계를 수행

    → *input > backbone > neck > dense prediction > sparse prediction*

### 📌 Training Step
- 데이터 증강(Data Augmentation -> 스케일링, 색 공간 조정, 모자이크 확대 등)
- 손실 계산(손실 함수를 계산하여 평균 정밀도를 제공)

### 📌 CSP BackBone
- DenseNet 기반
- 중복 그라디언트 문제를 해결하기 위해 비슷한 중요도에 대한 매개변수와 FLOPS를 축소한다.
    - 추론속도 증가, 모델 크기 감소

### 📌 PA-Net Neck

### 📌 YOLO v5 라벨링
- 형식 : TXT(`.coco`) 및 YAML(`.yml`)
- Tool : Roboflow
    - cf. VOTT, LabelImg, CVAT
```
torch.hub.load(“ultralytics/yolov5”, 'yolov5s').to('cuda')
```

### 🔥 YOLO v8
```
$ pip install ultralytics
```
➰　Pretrained Model Classes List
```
model = YOLO(“yolov8.pt”)
classes = model.model.names

YOLO(“yolov8n.pt”)
```

## 🎇 Reference
### 📌 Triton?
➰　NVIDIA Triton Inference Server (추론 서버)

    ⊙ 워크로드에서 AI 모델의 배포 및 실행을 표준화하기 위한
      오픈 소스 방식의 추론 제공 소프트웨어
    ⊙ 주요 프레임워크 지원
    ⊙ 동적 배치, 최적 실행 등 고성능 추론
    ⊙ DevOps 및 MLOps 솔루션 통합 설계
    ⊙ 지원, 보안 및 API 안정성
