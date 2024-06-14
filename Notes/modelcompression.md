---
tags:
  - DL
---
# Model Compression

    Network Compression

- 딥러닝을 이용해 해결하려는 문제가 복잡해지면서 모델 크기가 급격히 증가
    - Memory Limitation
    - Training/Inference Speed
    - Worse Performance
    - Practical Problems
- 따라서 다양한 방법을 이용하여 딥러닝 모델의 성능을 유지하며 크기를 줄이는 것을 목표로 하는 모델 압축이 중요해짐

## 모델 압축 방법론
### Pruning 가지치기
- 학습 후 불필요한 부분을 제거하는 방식
    - 가중치의 크기에 기반한 제거
    - 어텐션 헤드 제거
    - 레이어 제거
- layer dropout
    - prunability를 제거하기 위해 학습 중 정규화를 도입

### Weight Factorization 가중치 분해
- 가중치 행렬을 분해하여 두 개의 작은 행렬 곱으로 근사하는 방법
- low-rank constraint
    - 행렬이 낮은 랭크를 가지도록 하는 제약조건 도입
- 가중치 분해는 토큰 임베딩이나 feed-forward / self-atention layer 파라미터에 적용할 수 있음

### Knowledge Distillation 지식 중류
- 미리 학습된 큰 네트워크 Teacher Network로부터 실제로 사용하고자 하는 작은 네트워크 Student Network를 학습시키는 방식
    - 훨씬 작은 Transformer 모델을 pre-training/downstream-data에 대해 기본부터 학습시킴
    - fully-sized model의 값을 soft label로 사용하면 최적화 가능
- 빠른 추론 시간을 위해 BERT를 다른 형태의 모델(LSTM, etc.)로 증류하기도 함
- teacher를 더 자세히 분석하여 출력값 뿐만 아니라, 가중치 행렬이나 hidden activation 값을 사용하기도 함

### Weiget Sharing 가중치 공유
- 모델 일부 가중치들을 다른 파라미터들과 공유하는 방식

### Quantization 양자화
- 부동 소수점 값을 잘라 더 적은 비트만을 사용하는 방식
- 양자화된 값은 학습 과정 중에 배울 수도 있고, 학습 후에 양자화 될 수도 있음

### Pre-train vs. Downstream
- BERT를 특정 downstream task에만 맞게 압축
- 또는
- BERT를 task와 무관하게 압축

# ‘왜 처음부터 작은 모델을 만들어서 학습하지 않고 큰 모델을 만들어서 압축을 해야 하는가’

# Reference

[참조](https://blog.est.ai/2020/03/%EB%94%A5%EB%9F%AC%EB%8B%9D-%EB%AA%A8%EB%8D%B8-%EC%95%95%EC%B6%95-%EB%B0%A9%EB%B2%95%EB%A1%A0%EA%B3%BC-bert-%EC%95%95%EC%B6%95/)

<span style="color:pink;font-weight:600;"></span>
<span style="color:rgba(255,255,155,0.8);font-weight:500;font-style:italic;"> </span>
