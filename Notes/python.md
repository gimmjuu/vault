---
tags:
  - PYTHON
---
# Python✨
## 🎇 Generator
### 📌 제너레이터, 생성자
➰　메모리를 효율적으로 사용하는 Iterator(이터레이터, 반복자)

#### ⚡ `yield` : 제너레이터 생성 키워드
- 제너레이터 : 여러 개의 데이터를 미리 만들어 놓지 않고 필요할 때 하나씩 생성하는 객체 (<span style="color: rgb(255,98,98); font-weight: bold;">게으른 반복자iterator</span>)
- 결과값을 나누어 얻기 때문에 **성능** 측면에서 이점
- 결과값을 하나씩 메모리에 올려놓기 때문에 **메모리 효율** 측면에서도 이점

#### ⚡ `yield from list()` : 리스트를 바로 제너레이터로 변환
- 제너레이터 표현식 : abc = (char for char in "ABC")
    - 리스트 표현식과 다르게 <span style="color: rgb(255,255,125)">**소괄호**</span>로 표현
