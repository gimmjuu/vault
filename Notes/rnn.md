---
tags:
  - DL
  - RNN
---
# RNN

    순환 신경망 Recurrent Neural Network

## 기본 개념
- 자연어 문장처럼 토큰<span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;"> input data </span>의 순서에 따라 의미가 달라지는 순차 데이터<span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;"> Sequential Data</span> 처리에 주로 사용하는 신경망
- <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Hidden Layer</span> 노드에서 활성화 함수 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Activation Function</span>을 거친 결과값을 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Output Layer</span>로 보내는 동시에 다음 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Hidden Layer</span> 노드 입력값으로 보냄
  - ***순환신경망***

<details>
  <summary style="padding-bottom:10px;font-weight:500;font-style:italic;">🔥 Feed-Forward Neural Network</summary>
    
    피드 포워드 신경망
    
  <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Hidden Layer</span>에서 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Activation Function</span>을 통과한 결과 값이 오직 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Output Layer</span>로만 전달됨
</details>

<!-- <center> -->
<img src="../Assets/rnn-node.png" alt="RNN 도식화" width=500 style="border-radius:8px;"/>
<!-- </center> -->

<table>
  <thead>
    <tr>
      <th scope="col" style="text-align: center;">기호</td>
      <th scope="col" style="text-align: center;">뜻</td>
    </tr>
  </thead>
  <tbody style="text-align: center;">
    <tr>
      <td>$x$</td>
      <td>Input Layer의 입력 벡터</td>
    </tr>
    <tr>
      <td>$y$</td>
      <td>Output Layer의 출력 벡터</td>
    </tr>
    <tr>
      <td>$b$</td>
      <td>편향 bias</td>
    </tr>
    <tr>
      <td>$cell$</td>
      <td>Memory cell,<br>RNN cell</td>
    </tr>
  </tbody>
</table>

- $cell$
  - Hidden Layer에서  Activation Function을 거쳐 결과를 내보내는 역할
  - 이전 Time Step의 출력값을 기억하는 역할
    - ***Memory cell***
- $cell$이 값을 기억한다?
  - 이전 Time Step에서 Hidden Layer의 메모리셀 출력값을 자기 자신의 입력값으로 재귀적<span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Recursively</span>으로 사용한다는 뜻

<center>
<img src="../Assets/Recurrent_neural_network.png" alt="RNN 도식화" height=200 style="background-color: white; border-radius: 8px; margin-top: 10px;"/>
</center>

- Hidden State
  - 메모리 셀이 Output Layer 혹은 다음 Time Step의 메모리 셀에 보내는 값

- <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Time Step</span> $(t-1)$ 의 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Hidden State</span> $h_{(t-1)}$ 를 다음 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Time Step</span> $(t)$ 의 <span style="color:rgba(255,255,135,0.8);font-weight:500;font-style:italic;">Hidden State</span> 계산을 위한 입력값으로 사용

## 수식

<center>
<img src="../Assets/rnn3.png" alt="RNN 도식화" width=420 style="border-radius: 10px; margin-top: 10px;"/>
</center>

### $h_t = tanh(W_{xh}x_n + W_{hh}h_{t-1} + b_n)$
- $h_t$
  - $t$ 번째 Time Step에서의 Hidden State
- $W_{xh}$
  - 현재 입력값에 곱해지는 가중치
- $x_t$
  - 입력값
- $W_{hh}$
  - 이전 Time Step에서의 가중치
- $h_{t-1}$
  - $(t-1)$ 번째 Time Step에서의 Hidden State
- $b_h$
  - 편향
- $tanh$
  - 하이퍼볼릭 탄젠트, Activation Function 활성화 함수의 일종
  - <span style="color:rgb(255,255,175);font-weight:500;">Vanilla RNN</span> *기본적인 RNN* 은 활성화 함수로 하이퍼볼릭 탄젠트를 사용하는 것이 특징

## 구조
- 입력 벡터와 출력 벡터 길이에 따른 3가지 구조

### One-to-Many
<details>
    <summary style="padding-bottom:10px;font-weight:600;color:pink;">일대다 구조</summary>

┌───┐　┌───┐　┌───┐　┌───┐<br>
│　$y_0$　│→│　$y_1$　│→│　$y_2$　│→│　$y_3$　│<br>
└───┘　└───┘　└───┘　└───┘<br>
　　 ↑ 　　　　　 ↑ 　　　　　 ↑ 　　　　　↑<br>
┌───┐　┌───┐　┌───┐　┌───┐<br>
│$RNN$│→│ $RNN$│→│$RNN$│→ │$RNN$│<br>
└───┘　└───┘　└───┘　└───┘<br>
　　 ↑<br>
┌───┐<br>
│　$x_0$　│<br>
└───┘<br>
</details>

- 이미지 캡셔닝에 활용

### Many-to-One
<details>
    <summary style="padding-bottom:10px;font-weight:600;color:pink;">다대일 구조</summary>

.　　　　　　　　　　　　　　　　　　┌───┐<br>
　　　　　　　　　　　　　　　　　　 │　$x_0$　│<br>
　　　　　　　　　　　　　　　　　　 └───┘<br>
　　　　　　　　　　　　　　　　　　　　 ↑<br>
┌───┐　┌───┐　┌───┐　┌───┐<br>
│$RNN$│→│ $RNN$│→│$RNN$│→ │$RNN$│<br>
└───┘　└───┘　└───┘　└───┘<br>
　　 ↑ 　　　　　 ↑ 　　　　　 ↑ 　　　　　↑<br>
┌───┐　┌───┐　┌───┐　┌───┐<br>
│　$x_0$　│→│　$x_1$　│→│　$x_2$　│→│　$x_3$　│<br>
└───┘　└───┘　└───┘　└───┘<br>
</details>

- 감정 분류, 스팸 메일 분류(T/F)에 활용

### Many-to-Many
<details>
    <summary style="padding-bottom:10px;font-weight:600;color:pink;">다대다 구조</summary>

┌───┐　┌───┐　┌───┐　┌───┐<br>
│　$y_0$　│→│　$y_1$　│→│　$y_2$　│→│　$y_3$　│<br>
└───┘　└───┘　└───┘　└───┘<br>
　　 ↑ 　　　　　 ↑ 　　　　　 ↑ 　　　　　↑<br>
┌───┐　┌───┐　┌───┐　┌───┐<br>
│$RNN$│→│ $RNN$│→│$RNN$│→ │$RNN$│<br>
└───┘　└───┘　└───┘　└───┘<br>
　　 ↑ 　　　　　 ↑ 　　　　　 ↑ 　　　　　↑<br>
┌───┐　┌───┐　┌───┐　┌───┐<br>
│　$x_0$　│→│　$x_1$　│→│　$x_2$　│→│　$x_3$　│<br>
└───┘　└───┘　└───┘　└───┘<br>
</details>

- 기계 번역, 챗봇에 활용

# Reference
<!-- [참조](https://ctkim.tistory.com/entry/RNNRecurrent-Neural-Network?category=1097443) -->
[참조](https://heytech.tistory.com/440)
