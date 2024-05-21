---
tags:
  - python
  - pkg
  - ctypes
---
### [Python에서 C/C++ 동적 라이브러리 불러오기]

>파이썬3 내장 모듈 `ctypes` 불러오기

```python
from ctypes import *
```

```python
import ctypes
```

> 동적 라이브러리 호출

```python
windll.LoadLibrary("setupapi.dll")
```
