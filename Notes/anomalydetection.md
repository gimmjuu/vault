---
created:
  2024-06-20
tags:
  - API
---

<link rel="stylesheet" href="../Assets/style.css" />

# Index

---

# 이상 탐지 Anomaly Detection

- 적절한 모델을 통해 기준을 생성하고 이상 징후를 미리 감지해 경고를 보내며 근본적인 원인을 해결하고 대비하는 것

## Novelty / Outlier

- `Novelty`
    - 이전과는 다른 새로운 형태의 본 적 없는 데이터
    - Novelty Detection
- `Outlier`
    - 다른 데이터와 비교해 확연하게 다른 데이터
    - Outlier Detection

- 이상 탐지란 특이한 값, 보기 드문 사건을 사전에 탐지하는 기법
    - 비정상abnormal보다는 이상anomaly 찾기에 목적이 있음

## Novelty Detection vs Outlier Detection

- 어떤 데이터를 학습하고 어디서 이상 데이터를 찾을 것이냐

||정의|학습 데이터 특성|탐지 대상|이상 탐지|
|:-:|:-:|:-:|:-:|:-:|
|Novelty Detection|새로운 데이터가 학습된 데이터 분포에 포함되는지 여부를 판단|정상 데이터로만 구성|새로운 입력|Semi-supervised anomaly detection|
|Outlier Detection|학습 데이터 내에서 데이터들이 가장 많이 집중된 영역을 찾아내고 그 외의 데이터를 제거하는 것|정상/이상 데이터가 모두 존재|학습 데이터 내에서 제거|Unsupervised anomaly detection|

- 학습 데이터에 따라 Novelty가 결정됨
    - 학습 데이터를 어떻게 구성하는지에 따라 Novelty의 기준이 달라질 가능성이 큼
    - 오직 새롭게 입력되는 데이터를 대상으로 테스트를 진행함
- Outlier detection의 학습 데이터는 정상/이상 데이터가 모두 존재함
    - 주어진 학습 데이터에서 가장 많은 데이터가 집중된 영역을 찾아내 영역 밖의 데이터를 제거
    - 별도의 테스트 데이터가 필요하지 않음

> 이상 탐지는 정상/이상 데이터 간의 불균형이 매우 심한 분야  
> 무엇이 정상/이상 데이터인지 명확하게 구분하기 어려움  
> 따라서 현재 데이터에 맞춰 두가지 중 적절한 방법을 택하는 것이 좋은 이상 탐지를 위한 첫번째 단게

### Novelty Detection in Anomaly Detection

- 학습 데이터 분포를 찾아내어 새로운 데이터가 영역 내에 들어오면 정상 테이터로 간주

### Outlier Detection in Anomaly Detection

- `Local Outlier Factor`
- 학습 데이터 내에서 데이터 간 밀집도에 따라 군집화

## 이상 탐지에 사용하는 데이터

### 데이터의 종류

- 이상 거래 탐지 `fraud detection`
    - 보험, 신용, 금융 관련 데이터에서 불법 행위를 검출하는 문제
- 의료 이상 탐지 `medical anomaly detection`
    - 의료 영상, 뇌파 기록 등의 의학 데이터에서 이상치를 탐지하는 문제
- 산업 이상 탐지 `industrial anomaly detection`
    - 제품 외관 검사, 장비 결함 등 제조업 데이터에서 이상치를 탐지하는 문제
- 로그 이상 탐지 `log anomaly detection`
    - 시스템의 로그를 보고 문제, 실패 원인을 추적하는 문제

- 동시 사용 데이터 갯수에 따라,
    - 단변량 `univariate`
        - 하나의 데이터만 사용
    - 다변량 `multivariate`
        - 여러 개의 데이터를 동시에 사용
- 이상 탐지는 변수들 간의 연관성도 중요 요소로 작용하기 때문에, 단변량과 다변량을 구분하는 것이 중요함

### 시계열에서의 이상 데이터

- 시계열 데이터 `time-series`
    - CPU 사용량처럼 순차적으로 발생하는 연속적인 관측치
- 일반적으로 시계열 데이터는 값(평균, 최댓값, 최솟값) 자체도 중요하지만, 규칙성도 중요한 요소로 작용함
- 시계열 데이터의 특성상 비정상 데이터가 단기적 또는 장기적으로 나타나기도 함

<details>
    <summary class="_pinkbold">시계열에서의 이상 데이터 분류</summary>

<img src="https://image.toast.com/aaaadh/real/2023/techblog/05%281%29.png" alt="시계열 이상 데이터" class="_img80">
</details>

#### Point time-series anomaly

1. Global time-series anomaly
    - 전체 시계열을 고려했을 때 그 값이 정상 범주를 크게 벗어나는 데이터
2. Contextual anomaly
    - 인접한 시계열 데이터를 고려했을 때 약간의 변칙이 존재하는 데이터

#### Pattern time-series anomaly

<details>
    <summary class="_pinksmall">자주 사용되는 용어</summary>

- Shapelet
    - 시계열 데이터 중요 특성 시퀀스
- Cycle 순환성
    - 정해지지 않은 빈도, 기간으로 일어나는 상승 혹은 하락
- Trend 추세
    - 데이터가 장기적으로 증가하거나 감소하는 흐름
- Seasonality 계절성
    - 일정 <span class="_yellow">주기</span>를 가지고 반복되는 패턴
</details>

- Shapelet anomaly
    - 전체 시계열에 존재한느 일반적인 shapelet과 다른 shatelet, cycle을 가진 부분 시계열
- Seasonal anomaly
    - 모양이나 트렌드는 유사하지만 시계열 계절성에서 벗어나는 부분 시계열
- Trend anomaly
    - 시계열 추세에 영구적인 변화를 주는 부분 시계열

## 이상 탐지가 어려운 이유

### 도메인 지식

- 기본적으로 정상/이상 데이터를 구분하기 위해 무엇이 정상인지 파악해야 함
- 도메인에 대한 지식이 충분하지 않다면 정상/이상 데이터가 뒤섞이는 문제<span class="_yellow _small">anomaly contamination</span>가 발생함
    - 데이터 측정 과정에서도 노이즈와 손상이 발생하므로 전처리를 위해 충분한 도메인 지식이 필요함

### 데이터 불균형

- 일반적으로 이상 데이터는 매우 희귀하며 불명확함
- 매우 적은 양의 이상 데이터와 상대적으로 많은 양의 정상 데이터를 함께 혼합해 머신 러닝에 사용하면 성능이 나빠질 가능성이 매우 높음
- 일반적으로 비정상 데이터가 정상 데이터의 10%보다 적은 경우 단순 분류<span class="_yellow _small">classification</span> 머신 러닝을 권장하지 않음
- 이상 탐지는 보통 비정상 데이터가 극히 드물기 때문에 아직까지 정상 데이터만 학습시키는 비지도 학습이 대세임

---

# 이상 탐지 기법

## Statistical method

- 보기 드문 데이터를 찾기 위해 확률적으로 접근함

### Parametric statistical method

- 가우시안 분포, 정규분포
    - 세상을 설명하는 분포
    - 중심`m`에 확률`높이`이 집중되어 있고, 중심에서 멀어질수록 서서히 줄어드는 모양
- 몇 개의 확률 분포를 사용할 것인지에 따라,
    - Gaussian density estimation
        - 관측 데이터를 바탕으로 데이터의 실제 분포를 하나의 가우시안 분포에 근사하여 확률적으로 분석하는 방법
    - Mixture of gaussian density estimation
        - 하나가 아니라 다수의 가우시안 분포를 사용하는 gaussian density estimation
- 관측 데이터를 바탕으로 데이터의 실제 분포를 이미 우리가 알고 있는 확률 분포에 근사 하는 것
- 간단하게 데이터의 실제 분포를 예측할 수 있는 방법 중 하나

<details>
    <summary>3-sigma rule</summary>

- 가우시안 분포는 평균으로부터 3 표준편차(sigma) 범위 내에 거의 모든 값이 들어간다는 경험적 규칙
- 이상 탐지에서 Gaussian density estimation
    - 3 표준편차 마저 벗어나는 데이터를 보기 드문 사건<span class="_yellow _small">anomaly</span>으로 해석
    - 반드시 3 sigma를 적용할 필요는 없으며 세로축 값(확률)을 기준으로 탐지할 수도 있음
</details>

### Non-parametric statistical method

- Parametric statistical method의 치명적인 단점?
    - 미리 정해둔 확률 분포에 의해 결과가 크게 달라짐
    - 확률 분포를 정하기 어려움
- 데이터의 실제 확률 분포에 대해 사전 가장하지 않고 관측 데이터 그 자체만을 보는 방법이 제안됨
    - `Non-parametric statistical method`
- 대표적인 Non-parametric statistical method
    - histogram
        - 간격을 정해 구간에 들어오는 데이터를 계수
        - 비교적 간단하지만 확률 분포에 빈 공간이 발생함
    - kernel-density estimation
        - 데이터 하나하나에 확률 분포를 적용하는 방법
        - Mixture of gaussian density estimation을 데이터마다 적용하는 식
        - 연속적인 확률을 쌓아가는 과정이므로 확률 분포가 자연스레 퍼지면서 빈 공간 없이 데이터의 분포를 추측
    - 두 방식의 차이는 `연속성`
    - 어떤 확률과 어떤 측정 간격을 사용하는지에 따라 같은 관측 데이터에서도 다양한 결과를 얻을 수 있음

### 통계적 접근법의 단점

- 적절한 확률 분포를 찾기 힘듦
    - 적절한 확률 분포를 찾더라도 분포의 평균, 분산 등 고려할 파라미터가 많음
- 다차원 데이터에서 잘 동작하기 어려움
    - 다변량<span class="_yellowsmall">multivariate</span> 데이터를 다루기 어려움
    - 통계적 접근법에 국한되는 문제는 아니지만, 통계적 접근법에서 특히 다루기 어려움

## Machine learning


- 최근 개발되는 이상 탐지 모델 대부분이 머신 러닝 기반
- 대표적인 머신 러닝 기반 이상 탐지
    - 분류 `classification`
    - `nearest-neighbor`
    - `clustering`
    - `reconstruction-based` ← 현재 가장 대중적으로 쓰이는 방법

### Classification

- <span class="_yellowbold">분류</span> `경계를 긋는 일`
- 이상 탐지란 정상 데이터와 이상 데이터를 구분하는 경계를 찾는 이진 분류 문제
- 이상 탐지에서 분류를 적용하기에 `데이터 불균형`이라는 제약 사항이 큼
    - 그런데 분류는 데이터가 균등할 때 잘 동작하는 알고리즘!
    - 데이터 불균형을 해결하기 위해 하나의 클래스만 학습시키는 방법을 고안 <span class="_yellowsmall">But,</span> 활용도 낮음

<details>
    <summary class="_pink">Support Vector Machine</summary>

- 대표적인 분류 이상 탐지 모델
- 데이터 공간에 클래스별로 라벨링된 데이터를 두고, 클래스를 가장 잘 나눌 수 있는 경계면`hyperplane`을 찾는 것이 목적
- 고려사항
    - 얼마나 클래스를 잘 나누었는가
    - 경계면과 데이터가 충분히 떨어져`margin` 있는가
- `support vector`
    - hyperplane 형성에 가장 큰 영향을 주는 데이터
</details>

#### one class support vector machine `OCSVM`

- SVM으로 novelty detection을 하기 위한 방법론
- SVM과 동일한 동작 원리
- 하나의 클래스만 학습 + hyperplane을 형성하는 기준에 <span class="_yellowbold">원점</span>을 추가
- OCSVM의 목적?
    - 학습 데이터와 원점을 가장 잘 나눠 주는 hyperplane을 찾는 것
- 정상 데이터만을 학습해야 함

#### support vector data description `SVDD`

- OCSVM과 유사, hyperplane이 아닌 hypersphere를 찾는 것이 목적

#### OCSVM과 SVDD
- 정상 데이터만을 학습해야 함
- `kernel trick` 데이터 확장 기법 사용
    - 데이터의 차원을 확장하여 비선형 분류 문제를 선형 문제로 바꿔주는 기법
- 2차원 데이터를 kernel trick으로 3차원 확장하여 경계면<span class="_yellowsmall">Decision surface</span> 찾기의 난이도를 낮출 수 있음

<details>
    <summary class="_pinksmall">DeepSVDD</summary>

- SVDD와 딥러닝을 결합한 모델
- 2018년 발표 후 많은 연구의 베이스 라인으로 활용됨
</details>

#### 단점

- `라벨링된 데이터를 필요로 한다.`
    - 특히 OCSVM, SVDD는 정상 데이터만 학습해야 한다는 추가 제약사항 있음
- `비선형 분류 문제를 다루지 못 한다.`
    - kernel trick도 모든 상황에 대처할 수는 없음

### Nearest-Neighbor

- `정상 데이터라면 데이터 차원상 서로 밀집되어 있을 것이다.`
- 데이터 분포에 어떠한 가정도 하지 않음
- 데이터 자체만을 보기 때문에 모델 효율성에 따라 이상 탐지 성능이 크게 달라짐
- 이웃 데이터 간 거리<span>distance</span> 정보와 밀도<span>density</span> 정보를 활용하여 데이터마다 점수를 부여하고 임곗값을 넘으면 이상 데이터로 간주함
- Nearest-Neighbor의 방법론
    1. Distance-based
        - 이웃한 데이터간 거리 정보를 고려하여 점수를 부여
        - `K-Nearest Neighbor (KNN)`
    2. Density-based
        - 이웃한 데이터간 밀도를 고려하여 점수를 부여
        - `Local Outlier Factor`

<details>
    <summary class="_pinksmall">KNN</summary>

- 거리 기반 이상 탐지의 대표적인 예시
- 인접한 K개의 데이터까지 거리의 평균, 합, 최댓값, 최솟값 등을 고려해 데이터에 점수를 부여
- 점수가 임곗값을 넘으면 이상 데이터로 간주
</details>

#### 거리 기반 이상 탐지의 단점

- 임곗값보다 가까운 outlier를 정상 데이터로 판단할 가능성이 높음
- 이를 보완하는 방법론으로 Local Outlier Factor(LOF) 제안

### Clustering

- Nearest-Neighbor와 유사 <span class="_yellowsmall">But,</span> 최종 목표는 군집을 찾는 것
- `이상 데이터는 어느 군집에도 속하지 않을 것이다.`
- 대표적인 Clustering
    - `K-Means`

<details>
    <summary class="_pinksmall">K-Means</summary>

- 장점
    - 라벨링된 데이터가 필요하지 않음(Nearest-Neighbor와 동일)
- 단점
    - 군집의 개수를 미리 지정해야 함
    - 알고리즘 효율성에 따라 성능 차이가 큼
</details>

#### Clustering 이상 탐지의 맹점

- 대부분의 군집화 알고리즘은 모든 데이터가 어떤 군집이든 무조건 포함되도록 함
    - 이상 탐지의 취지와 배치됨

### Reconstruction-based

- 현재 이상 탐지의 대세 중 하나
- 주어진 데이터에 숨어 있는 유효한 특성<span class="_yellowsmall">feature</span>을 추출한 후 다시 데이터를 복원했을 때,
    - 정상 데이터는 본래대로 복원됨
    - 비정상 데이터는 제대로 복원되지 않음
- 대표적인 방법
    - `principal component analysis (PCA)`

<details>
    <summary class="_pinksmall">PCA</summary>

- 본래 고차원 데이터를 다루기 위한 차원 축소법
    - 주어진 데이터를 분산이 가장 커지는 축으로 환원하는 기법
    - 데이터를 축소하는 변환 정보를 저장해 본래 데이터를 어느 정도 다시 복원할 수 있음
- 기본 개념을 응용해 `비정상 데이터라면 제대로 복원되지 않을 것`이라고 가정
    - <span class="_yellow">★</span> 데이터를 축소할 때 정상 데이터만 사용해야 함
</details>

#### PCA의 한계 

- 선형 변환
- 다차원 데이터 간 연관성을 다루기 부족함
- 이를 보완하기 위해 뉴럴 네트워크 기반의 `Autoencoder`를 사용

<details>
    <summary class="_pinksmall">Autoencoder</summary>

- 기본적으로 데이터를 압축한 후 복원 + 비선형 변환이 가능
- 성능이 입증된 모델 → 최신 모델에도 많이 적용
</details>

## 고전 머신러닝 VS 딥러닝

### Deep Learning Overview

- 기존 모델은 대부분 고전 머신러닝 모델들이지만, 현재는 딥러닝 기반 모델도 많이 사용되고 있음
- 딥러닝 기반 모델이 증가한 이유
    - 데이터 양 증가
    - 데이터 차원 및 차원 간 연관성 고도화
    - 고전 머신 러닝 모델 성능의 한계 도달
- 따라서 고전 머신 러닝의 개념과 딥 러닝을 결합한 모델 연구가 활발하며 성능이 뛰어난 모델도 있음

### Deep Learning for Time-series Anomaly Detection

**시계열 이상 탐지에서의 딥 러닝**

- 시계열 데이터 이상 탐지의 추가 고려사항
    1. 데이터 차원 간의 연관성
    2. 데이터 시간 축에서의 연관성<span class="_yellow">★★</span>
- 시간 축 연관성을 찾아내기 위해 사용하는 딥 러닝 모델(시퀀스 데이터에 특화된 모델)
    - TemporalCNN
    - RNN
    - Transformer
- 현재 시계열 이상 탐지는 데이터 불균형, 라벨링 데이터 부족 등 정상 데이터만 학습하는 <span class="_pinkbold">비지도 학습</span>이 주류임
    1. `Prediction-based`
    2. `Reconstruction-based`

## Deep-learning

- <span class="_yellowbold">Prediction-based</span>
    - 주어진 시계열을 분석하여 다음에 올 값을 예측하고 실제 값과의 차이를 시계열 이상 탐지에 사용하는 방법
    - 예측을 시작한 시점에서 시간이 지날수록 오차가 점차 누적될 가능성이 있음
- <span class="_yellowbold">Reconstruction-based</span>
    - 예측과 달리 주어진 시계열 그 자체를 그대로 복원하여 그 차이를 시계열 이상 탐지에 사용하는 방법
    - 조금더 안정적인 성능을 보여줌

- `두 접근법은 큰 의미에서 같은 분석이 가능하다.`
    - 어떤 네트워크(NLP, RNN, Transformer 등)를 사용해 시계열 데이터의 특성을 추출할 것인가
    - 어떤 아이디어를 결합하여 성능을 높일 것인가
    - 추출한 특성<span class="_yellowsmall">feature</span>으로 어떤 결과를 만들어낼 것인가
        - 본래값 <span class="_pinkbold">reconstruction</span>
        - 다음값 <span class="_pinkbold">prediction</span>

<details>
    <summary class="_pinksmall">Major Deeplearning Model 3</summary>

|방법|네트워크|모델|
|:-:|:-:|:-:|
|Prediction|RNN|THOC|
|Reconstruction|VAE|SISVAE|
|Reconstruction|Transformer|Anomaly Transformer|
</details>

### Deep Learning Network (major3)

#### Recurrent Neural Network (RNN)

- 순환 신경망 `RNN`
    - 시퀀스 데이터에 특화된 모델
- 데이터를 순차적으로 네트워크에 입력하여 <span class="_italic _pinkbold">Hidden State</span> 결과값을 얻어냄
    - hidden state는 다음 입력 데이터와 함께 다시 네트워크에 입력되어 다음 결과값을 얻어냄
- 과정이 순환하며 동작하므로 순환 신경망이라고 함

#### Variational Auto Encoder (VAE)

- 오토 인코더(Autoencoder)와 변분추론(Variational inference)을 결합한 모델
    - 오토 인코더
        - 입력 데이터를 압축하여 다시 입력을 복원하는 기법
    - 변분추론
        - 모델링하기 어려운 사후 확률을 이미 알고 있는 확률 분포에 근사하여 추론하는 기법
- 오토 인코더와 구조적으로 유사하지만 확률적 가정이 추가되어 데이터 분포를 모델링할 수 있다는 차별점을 가짐
- 구조적 유사성에도 불구하고, 두 모델은 명확히 구분됨<span class="_yellow">★</span>
    - 오토 인코더는 주어진 데이터를 특정 공간으로 잘 투영하는 법을 학습 → `Manifold learing`
    - VAE는 데이터 특성을 확률적으로 분석하여 추출 → `Generative model`

#### Transformer

- RNN 역할(시퀀스 데이터 특화)을 수행하면서 RNN의 단점은 해결하는 네트워크

<details>
    <summary>RNN의 단점</summary>

1. 순차적으로 진행되기 때문에 시간이 너무 많이 걸림
2. 순환 구조로 인해 기존 추출 정보가 서서히 사라짐
</details>

- Transformer의 self-attention 메커니즘
    - <span class="_yellowbold">시퀀스 데이터 내부 연관성 정보</span>를 뛰어난 성능으로 한번에 추출할 수 있음

### Deeplearning for Anomaly Detection

#### THOC (Temporal Hierrarchical One-Class Network)

<details>
    <summary class="_pinksmall">시간 계층 단일 클래스 네트워크</summary>

- <span class="_small _600">Temporal</span>
    - `RNN 네트워크로 시계열 데이터에서 특성을 추출한다.`
- <span class="_small _600">Hierarchical</span>
    - `특성을 다단계로 뽑는다.`
- <span class="_small _600">SVDD(One-Class)</span>
    - `뽑아낸 특성들은 서로 유사성을 갖는다.`

- 딥러닝 네트워크가 학습되면서 입력 데이터를 기반으로 유용한 특성을 추출하고<span class="_yellow _italic">feture extraction</span>, 표현을 학습하여<span class="_yellow _italic">representation learning</span> 좋은 성능을 낼 수 있음
- 네트워크를 사용해 좋은 특성, 표현을 찾아내고 해당 정보를 바탕으로 다시 특성, 표현을 추출하면 더 깊고 의미있는 정보를 얻을 수 있음 → `Hierarchical`
</details>

- `THOC`는 시계열 데이터를 `RNN`에 입력하여 특성<span class="_yellowsmall">feature</span>을 추출
- 추출한 특성들로 새로운 시계열 데이터를 생성하여 다시 RNN에 입력하는 과정들을 반복 → <span class="_pinkbold">Temporal Hierarchical</span>
- 계층적으로 특성을 추출
- RNN으로 추출한 특성들이 feature space 내에 밀집해 있을 것이라고 가정하여 `SVDD`를 적용 → <span class="_pinkbold">One-Class</span>

#### SISVAE (Smoothness-Inducing Sequential Variational Auto-Encoder)

<details>
    <summary class="_pinksmall">평활도 유도 순차 가변 자동 인코더</summary>
    
- <span class="_small _600">Smoothness</span>
    - `RNN 네트워크로 시계열 데이터에서 특성을 추출하고 VAE로 입력 데이터의 분포(평균, 분산)를 복원한다. 정상 데이터라면 분포가 부드럽게 이어질 것이다.`
</details>

- `SIS-VAE`
    - Reconstruction-Method
    - RNN, VAE를 사용하여 입력 데이터를 복원
        - 값 자체가 아닌 값의 확률 분포를 복원
    - `정상적인 시계열 데이터는 급격하게 변하지 않을 것이다.`
        - `인접한 데이터들의 확률 분포는 부드럽게 이어질 것이다.`
    - 시간 축에서 데이터 변화의 부드러움을 추구함

#### Anomaly Transformer

<details>
    <summary>이상 탐지 트랜스포머</summary>

- <span class="_small _600">Transformer</span>
    - `Transformer로 시계열 데이터 특성을 추출한다. Transformer로 추출한 특성에서 입력 시계열을 복원한다. 함께 추출한 시계열 축 데이터 연관성 분포를 확률 분포에 근사한다.`
</details>

- `Anomaly Transformer`
    - 시계열 데이터에 Transformer를 접목 + 트랜스포머로 얻은 시퀀스 데이터 내 연관율 분포를 이미 알고 있는 확률 분포에 근사
        - 다양한 확률 분포 중 `가우시안 분포`가 가장 좋은 결과를 보임
    - `정상 데이터의 경우 특정 지점을 기준으로 연관성을 추출한다면 균등한 분포 모양을 보일 것이다.`
        - `이상 데이터의 경우 전조증상과 흔적을 남겨 인접한 데이터와 높은 연관성을 가질 것이다.`
    - 데이터 연관성의 균일함을 추구

### 딥러닝 모델의 실제 적용

- 시계열 데이터의 주안점
    1. 데이터 누락, 측정 오류 수정
        - 누락 데이터를 0이나 평균값으로 대체
        - 전후 데이터로 누락 데이터를 예상하여 Regression으로 보강
        - 누락 데이터를 Anomaly로 판단
    2. 시계열 정상성(Stationary) 확인
        - **정상성**이란, "시간이 변해도 특성이 일정한 시계열"
        - <span class="_yellowbold">시계열 데이터의 특성</span> 평균, 분산, 트렌드 등
        - 비정상성 시계열을 정상성 시계열로 변환하는 게 가장 중요
            - <span class="_yellowsmall">유효 전처리 과정</span> Differencing(차분), Detrending, Deseasonalizing 등 → 시계열 데이터의 비정상성을 제거
        - 데이터를 변환하는 과정은 고비용 + 데이터 고유 특성의 유실 가능성이 있음
            - 원본 시계열 데이터를 그대로 보존하여 Normalization(정규화)만 적용하기도
            - 시계열 데이터에 특화된 정규화 → Sliding-Window Normalization

### 이상 탐지의 도전 과제

- 정확한 이상 탐지를 위한 좋은 모델
    - 도메인에 대한 충분한 이해와 적절한 기법 + 아이디어
- 측정된 anomaly score를 바탕으로 적절한 임계값을 설정
    - 데이터마다 임계값이 모두 다를 수도 있음
    - 방법 1. 규칙을 설정해 고정값을 임계치로 설정
    - 방법 2. Extreme Value Theory (극한치 분포)
    - 방법 3. Gap Statictic Method 등

# Python Ribrary

## PyOD

[Github](https://github.com/yzhao062/pyod)

### Installation

```bash
pip install pyod            # normal install
pip install --upgrade pyod  # or update if needed
```

### Model Save & Load

- Recommand to user
```python
from joblib import dump, load

# save the model
dump(clf, 'clf.joblib')
# load the model
clf = load('clf.joblib')
```

### Outlier Detection

```python
# Example: Training an ECOD detector
from pyod.models.ecod import ECOD

clf = ECOD()
clf.fit(X_train)
y_train_scores = clf.decision_scores_  # Outlier scores for training data
y_test_scores = clf.decision_function(X_test)  # Outlier scores for test data
```

