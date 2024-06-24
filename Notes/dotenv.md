---
created:
  2024-06-19
tags:
  - DEVELOP
  - CONFIGURATION
---

<link rel="stylesheet" href="../Assets/style.css" />

# dotenv

dotenv로 환경변수 관리하기

## .env 파일 작성

- 명령어 export로 적용한 환경변수는 현재 터미널을 유지하는 동안 임시로 사용하게 됨
- 환경변수를 Linux 운영체제에 저장하는 방법의 일종
    - `.env` 파일 작성

### `.env` 파일 형식

```conf
KEY=VALUE
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```

## `dotenv`
- `dotenv` 라이브러리는 default 값으로 현재 디렉토리의 .env 파일로부터 환경변수를 로드
- 해당 라이브러리는 원래 <span class="_yellow">npm</span> 의존성 모듈이지만,
  - <span class="_pink _600">Python</span>을 위해 리팩토링된 `python-dotenv` 라이브러리가 있음

### `python-dotenv`

```bash
pip install python-dotenv
```

```python
from dotenv import load_dotenv

load_dotenv()
```

```python
import os

print(os.environ["LANGCHAIN_TRACING_V2"])
```

```console
true
```
