---
aliases:
  - Horizontal
cssclasses:
  - Invalid
tags:
  - python
---

# Python psutil

➰　주로 시스템 모니터링, 프로파일링, 프로세스 리소스 제한 및 실행 중인 프로세스 관리에 유용

➰　psutil은 내장 모듈이 아니기 때문에, pip를 이용해 설치

## System related fuctions

### 📌CPU

➰　cpu 시간을 튜플로 리턴

➰　percpu가 True로 되면 시스템의 각 cpu에 대해 튜플로 반환

```python
psutil.cpu_times(percpu=False)
```

➰　현재 시스템의 전체 cpu 사용률을 백분율로 반환

➰　interval 옵션의 경우 최소 0.1에서 몇초간 호출

➰　0이나 none의 경우 의미없는 값을 반환

➰　percpu가 True이면 시스템 각 cpu의 사용률을 백분율로 반환

```python
psutil.cpu_percent(interval=None, percpu=False)
```

➰　cpu 시간에 대한 사용율

```python
psutil.cpu_times_percent(interval=None, percpu=False)
```

➰　logical 플래그가 False인 경우 물리적 코어의 수를 반환

➰　True인 경우 논리적 코어의 수를 반환

```python
psutil.cpu_count(logical=True)
```

### 📌Memory

```python
psutil.virtual_memory()
```
