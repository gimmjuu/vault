---
tags:
  - DL
  - PYTHON
---

# Hugging Face 🤗

## 🎇 Python package `transformers`
### 📌 Install

    $ pip install transformers

### 📌 Pipeline
➰　pipeline 함수는 허깅페이스에 업로드된 모델을 불러오는 API 기능을 제공함

    from transformers import pipeline

### 📌 Pre-trained 모델 사용하기
➰　허깅페이스 페이지에서 해당 모델의 Task와 주소를 pipeline 함수에 입력하면 해당 모델을 객체로 호출함

    client = pipeline("{Task}", "{model-address}")
    result = client(target-data)

### 📌 모델 로컬에 다운받기
➰　허깅페이스 페이지에서 해당 모델의 File and Versions 탭으로 이동<br>
➰　업로드된 파일을 전체 다운로드 후, pipline 함수 호출 시 다운 받은 로컬 경로를 지정해줌
    
    client = pipeline(“{Task}”, model=’C:/path/to/models/target-model’)