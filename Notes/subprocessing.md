---
tags:
  - PYTHON
  - PKG
---
# subprocessing
## 🎇 python library `subprocessing`
- 현재 소스코드 안에서 다른 프로세스를 실행해주며 그 과정에서 데이터의 입출력을 제어하기 위해 사용
- 기존 `system`, `os` pkg를 사용하던 기능을 <span style="color: rgb(255,97,97); font-weight: bold;">Python 3</span>부터 보안상의 이유로 `subprocess`로 이관
    - ➰　`subprocess` 모듈은 새로운 프로세스를 실행하도록 도와주고 프로세스의 입출력 및 에러 등 리턴 코드를 유저가 직접 제어하게 해주는 모듈이다.
- <span style="color: rgb(255,97,97); font-weight: bold;">Python 3.5</span>부터 `subprocess.run()` 메소드를 통해 `subprocess`를 실행

## 🎇 1. 쉘 명령어 실행
```python
import subprocess
```

### 📌 Linux
```python
subprocess.run([‘ls’], shell=True, check=True)
```
### 📌 Windows
```python
subprocess.run([‘dir’], shell=True, check=True)
```

## 🎇 2. 파이썬 소스코드 실행
```python
import subprocess

subprocess.run([‘test.py’], shell=True)
```

### 📌 실행 결과 반환
➰　`subprocess.check_output()` 메소드를 사용해 커맨드 실행 결과를 메모리상 저장하고 반환
```python
import subprocess

result = subprocess.check_output([‘test.py’], shell=True, encoding=’utf-8’)
```

## 🔥 `run` 메소드의 <span style="color: rgb(255,97,97); font-weight: bold;">args</span> 인수
```python
import sys

result = subprocess.run(args=[sys.executable, ‘test.py’], capture_output=True)
```

## ⚡ Popen
➰　유연한 서브 프로세스를 실행하고 결과를 반환
