---
tags:
  - DL
  - LSTM
---
# LSTM



## RNN의 한계
- RNN `Recurrent Neural Network`?
  - 이전 입력 데이터를 기억해 다음 출력 값을 결정하는 모델
- <span style="color: pink;font-weight: 600;">But</span> 입력 데이터의 길이가 길어지면 그래디언트 소실 문제`Gradient Vanishing Problem`, 장기 의존성 문제`Long-Term Dependencies`가 발생
  - RNN의 그래디언트 소실 문제를 해결하기 위해 고안된 모델 → LSTM

## `LSTM`
- Long Short-Term Memory
- RNN 모델의 일종
- 시계열 데이터를 처리하거나 시퀀스 정보를 사용해 작업을 수행하는데 사용
- 이전 정보를 오랫동안 기억할 수 있는 메모리 셀을 통해 긴 시퀀스 데이터를 처리
  - 시계열 데이터의 예측, 자연어 처리, 음성 인식, 이미지 분류 등에 사용

## LSTM 구조
- 메모리 셀 `Memory Cell`
  - 입력값과 이전 상태에 따라 값을 업데이트, 새로운 상태를 출력
- 게이트 `Gate`
  - 셀의 값을 얼마나 기억할지 결정
  - 이를 통해 필요한 정보만 기억하도록 제어

<img
  src="/Assets/lstm_cell_structure.png"
  alt="LSTM 메모리 셀 구조"
  width="400"
  height="200"
  style="margin-left: 50px;"/>

## LSTM 동작 원리
### `Cell State`
- 이전 상태에서 현재 상태까지 유지되는 정보의 흐름
- 오래된 정보를 기억하고 새로운 정보를 적절히 갱신함
- Cell State는 셀 상단에 위치하며 자기 자신에게 피드백됨
- <span style="background: rgba(255,255,135,0.2); font-weight: 600;">① 망각 게이트 ② 입력 게이트</span> 두 번 변화를 겪음
  - 결과 Cell State 값 *`C`<sub>`t`</sub>* → 다음 입력으로 들어가는 값
### 망각 게이트 `Forget Gate`
- 이전 셀 상태의 정보를 지울 것인지 말 것인지 결정
- 망각 게이트 출력 신호 *`f`<sub>`t`</sub>*
  - 활성화 함수 `Sigmoid`로 계산
  - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;">f<sub>t</sub> = σ(W<sub>χf</sub>h<sub>t-1</sub>+W<sub>χf</sub>χ<sub>t</sub>)</span>
- Sigmoid 결과값
  - 0 <= *return* <= 1
  - 값이 `0`이면 셀 상태 정보 소멸
  - 값이 `1`이면 셀 상태 정보 전달

### 입력 게이트 `Input Gate`
- 새로운 정보를 어떻게 반영할 것인지 결정
  - 이를 통해 기존 정보와 새로운 정보를 적절히 조합하여 예측 정확도를 높임
- 활성화 함수 `Sigmoid`
  - 후보값을 얼마나 전달할지 결정하는 값(0과 1 사이 값)
  - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;">i<sub>t</sub> = σ(W<sub>hi</sub>h<sub>t-1</sub>+W<sub>χi</sub>χ<sub>t</sub>)</span>
- 활성화 함수 `tanh`[^tanh]
  - RNN과 동일한 출력 계산 방식
  - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;">C̃<sub>t</sub> = tanh(W<sub>hc</sub>h<sub>t-1</sub>+W<sub>χc</sub>χ<sub>t</sub>)</span>
- Sigmoid 값`i`<sub>`t`</sub>과 tanh 값`Ĉ`<sub>`t`</sub>을 곱해서 셀 상태`C`<sub>`tf`</sub>에 더해 최종 `C`<sub>`t`</sub>를 산출
  - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;">C<sub>t</sub> = C<sub>tf</sub> ⊕ i<sub>t</sub> ⊗ C̃<sub>t</sub></span>
  - `⊕` 배타적 논리합
  - `⊗` 텐서곱

### 출력 게이트 `Output Gate`
- Cell State 값에 tanh 함수 적용 값을 출력으로 사용
- 은닉 상태와 현재 입력에 대해 Sigmoid 적용 → 출력값의 중요도 조절
  - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;">O<sub>t</sub> = σ(W<sub>ho</sub>h<sub>t-1</sub>+W<sub>χo</sub>χ<sub>t</sub>)</span>
- 출력 신호와 출력 중요도를 텐서곱하여 출력 산출
  <!-- - <span style="font-style: italic; letter-spacing: 2px; font-weight: 600; color: pink;"> -->

## LSTM 코드 구현
```python
import tensorflow as tf
import matplotlib.pyplot as plt
from tensorflow.keras.datasets import imdb
from tensorflow.keras.preprocessing import sequence
from tensorflow.keras import layers

# Load the IMDB dataset
(x_train, y_train), (x_test, y_test) = imdb.load_data(num_words=10000)

# Pad the sequence to the same length
max_len = 500
x_train = sequence.pad_sequences(x_train, maxlen=max_len)
x_test = sequence.pad_sequences(x_test, maxlen=max_len)

# Build the model
model = tf.keras.Sequential()
model.add(layers.Embedding(10000, 128))
model.add(layers.LSTM(128, dropout=0.2, recurrent_dropout=0.2))
model.add(layers.Dense(1, activation='sigmoid'))

# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# Train the model
history = model.fit(x_train, y_train, batch_size=32, epochs=10, validation_data=(x_test, y_test))

# Plot the loss and accuracy results
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.title('Model Loss')
plt.ylabel('Loss')
plt.xlabel('Epoch')
plt.legend(['Train','Validation'], loc='upper right')
plt.show()

plt.plot(history.history['accuracy'])
plt.plot(history.history['val_accuracy'])
plt.title('Model Accuracy')
plt.ylabel('Accuracy')
plt.xlabel('Epoch')
plt.legend(['Train','Validation'], loc='lower right')
plt.show()
```

## LSTM 수위 보정 알고리즘
### 1. 데이터 셋
- 시계열 데이터 수집
- 데이터 전처리(결측치 처리, 이상치 제거, 정규화 등)
  - 데이터 정규화
    - $X = \frac{X-X_{min}}{X_{max}-X_{min}}$
- LSTM 입력 데이터 형태로 변환
  - 주어진 시점 $t$ 에서의 입력 데이터를 $x_{t-i}$ 부터 $x_{t-1}$ 까지의 시계열 데이터로 정의
  - 입력 데이터 $X$
    - $X_{t} = [x_{t-n}, x_{t-n+1},...,x_{t-1}]$
  - 출력 데이터 $Y$
    - $Y_{t} = x_{t}$
  - 이때, $n$은 Time Step
- train, validation, test 데이터 분할

### 2. LSTM 모델 구축
- LSTM 기본 구성 요소
  - 입력 게이트 $i_t$
  - 망각 게이트 $f_t$
  - 출력 게이트 $o_t$
  - 셀 상태 $C_t$
  - 은닉 상태 $h_t$

- ① $f_t = σ(W_f⋅[h_{t-1},x_t]+b_f)$
- ② $i_t = σ(W_i⋅[h_{t-1},x_t]+b_i)$
- ③ $C̃_t = tanh(W_c⋅[h_{t-1},x_t]+b_c)$
- ④ $C_t = f_t⋅C_{t-1}+i_t⋅C̃_t$
- ⑤ $o_t = σ(W_o⋅[h_{t-1},x_t]+b_o)$
- ⑥ $h_t = o_t⋅tanh(C_t)$

||기호| |의미|
|:---:|:---:|:---:|:---|
||$σ$| |활성화 함수 `Sigmoid`|
||$tanh$| |활성화 함수 `Hyperbolic Tangent`|
||$W$| |학습 가능한 가중치|
||$b$| |편향|

- 모델 학습 손실함수 `MSE`
  - 평균 제곱 오차
  - $MSE = \frac{1}{N}∑^N_{i=1}(Y_i - Ŷ_i)^2$
  - $Y_i$ 실제 값
  - $Ŷ_i$ 예측 값

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import LSTM, Dense

# --데이터 정규화
scaler = MinMaxScaler(feature_range=(0, 1))
scaled_data = scaler.fit_transform(data)

# --데이터 준비
def create_dataset(data, time_step=1):
    X, Y = [], []
    for i in range(len(data)-time_step-1):
        a = data[i:(i+time_step), 0]
        X.append(a)
        Y.append(data[i + time_step, 0])
    return np.array(X), np.array(Y)

time_step = 10
X, Y = create_dataset(scaled_data, time_step)

# LSTM 입력 형태로 변환 [samples, time steps, features]
X = X.reshape(X.shape[0], X.shape[1], 1)

# --LSTM 모델 구성
model = Sequential()
model.add(LSTM(50, return_sequences=True, input_shape=(time_step, 1)))
model.add(LSTM(50, return_sequences=False))
model.add(Dense(1))

model.compile(optimizer='adam', loss='mean_squared_error')

# --모델 학습
model.fit(X, Y, epochs=100, batch_size=32, validation_split=0.2, verbose=1)
```

#### `^` Circumflex Hat | 곡절기호 햇
- 수학에서,
  - 변수 이름 수정
- 기하학에서,
  - 각도 표현
- 벡터 표기법에서,
  - 단위 벡터(unit vector): 크기가 1인 무차원 벡터
  - ex,데카르트 좌표계에서 $x$축 방향 단위 벡터
- 통계학에서,
  - 추정량 혹은 추정값

### 예측 및 보정
- 예측값을 원래 스케일로 역정규화
  - $X̂ = X_{min} + X̂'⋅(X_{max}-X_{min})$
  - $X̂$ 역정규화된 예측값
  - $X̂'$ 정규화된 예측값

```python
# --예측 수행
train_predict = model.predict(X_train)
test_predict = model.predict(X_test)

# 원래 스케일로 변환
train_predict = scaler.inverse_transform(train_predict)
test_predict = scaler.inverse_transform(test_predict)

# --결과 시각화
plt.figure(figsize=(10,6))
plt.plot(data.index, scaler.inverse_transform(scaled_data), label='Actual Data')
plt.plot(train_predict_index, train_predict, label='Train Predict')
plt.plot(test_predict_index, test_predict, label='Test Predict')
plt.legend()
plt.show()
```

### 모델 평가 및 보정 적용
- 모델 성능 평가
  - 예측값과 실제값 간의 `RMSE`(Root Mean Squared Error) 계산
  - $RMSE = \sqrt{\frac{1}{N}∑^N_{i=1}(Y_i - Ŷ_i)^2}$

```python
from sklearn.metrics import mean_squared_error

train_rmse = np.sqrt(mean_squared_error(y_train, train_predict))
test_rmse = np.sqrt(mean_squared_error(y_test, test_predict))

print(f'Train RMSE: {train_rmse}')
print(f'Test RMSE: {test_rmse}')
```

- LSTM 모델의 예측값을 이용하여 실시간 수위 보정을 수행

## Reference

[참고 1: LSTM 원리와 수식 연산](https://docs.likejazz.com/lstm/#fn:fn-3)

[참고 2: Chris Olah](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

[참고 3: 수학 기호](https://www.rapidtables.org/ko/math/symbols/Basic_Math_Symbols.html)

[^tanh]: [HyperbolicTangent]

[HyperbolicTangent]: https://mathworld.wolfram.com/HyperbolicTangent.html