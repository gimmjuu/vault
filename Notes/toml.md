---
tags:
  - CONFIGURATION
  - PYTHON
---
# `.toml`

    Tom's Obvious Minimal Language

- 구성 데이터 저장 목적
- 인라인 주석 지원(`#`, ↔ json은 미지원)
- `pyproject.toml`

### 주석 기호 `#`
- 해시 기호 이후 라인 끝까지 주석 처리
- 키 또는 값 문자열 자체에 포함되는 해시 기호는 해당없음

```toml
key = value # a comment after a k-v
# this is a comment
```

## TOML 기본 형식
- <span style="color: rgb(255,135,255);">Key = Value</span> 키값쌍

### 키
- 항상 문자열로 해석

### 값
- 문자열 `name = "string"`
- 정수 `integer = 3`
- 실수 `float-value = 3.141592`
- 부울 `boolear = true`
- 날짜-시간 값 `date = "OK"`
- `"quoted ünicode key" = true`
- <span style="background: rgba(255, 255, 0, 0.1);">배열</span>
- <span style="background: rgba(255, 255, 0, 0.1);">인라인 테이블</span>

#### 배열
- 하나의 키에 여러 값을 할당
- 값의 형식이 같을 필요는 없음
- 하나의 정의에 여러 줄을 사용할 수 있음
- 파이썬에서 배열은 리스트`list`에 직접 매핑

```toml
int-values = [1, 2, 4, 8, 16]
strings = ["prime", "audio", "soup"]
mixed = [1, 2, "Three", 4.0]
multi-line = [
    "array",
    "of",
    "strings"
]
```

#### 테이블
- 키값쌍의 모음
- 헤더 레이블(별도의 네임스페이스)로 구분
- 파이썬에서 테이블은 중첩된 딕셔너리`dict`로 취급

```toml
[general]
make_network_connect = true
ping_time = 1200

[user]
default_name = "Anonymous"
ping_time = 1600
```

##### 네임스페이스를 만드는 다른 방법
1. 점으로 구분된 이름
```toml
general.make_network_connection = true
general.ping_time = 1200

user.default_name = "Anonymous"
user.ping_time = 1600
```
2. 인라인 테이블
```toml
general = {make_network_connection = true, ping_time = 1200}
user = {default_name = "Anonumous", ping_time = 1600}
```

#### 테이블 배열
- 중첩된 구조 생성
```toml
[[movies]]
name = "Blade Runner"
year = 1982
[[movies]]
name = "Blade Runner 2049"
year = 2017
```
- cf. json과 유사
```json
movies = {
    {name: "Blade Runner", year: 1982},
    {name: "Blade Runner 2049", year: 2017}
}
```

## 파이썬에서 `.toml` 사용하기
### `tomlib`
- TOML 표준 라이브러리 모듈(<span style="color: rgb(255,255,135);"><= Python 3.11</span>)
- toml을 읽고 파이썬 객체 (`dict`)로 파싱하는 파이썬 네이티브 방법 제공
- 파이썬 객체를 toml로 직렬화 불가
    - 사실상 읽기 작업 전용!
```python
import tomlib
```

### tomli-w
- 서드 파티 솔루션
- 파이썬 dict를 toml로 직렬화 하기 위한 최소 라이브러리
- toml 유효성 자동 검증 기능이 없기 때문에 작성된 데이터에 대한 수동 확인이 필요함

```bash
pip install tomli-w
```

1. write to file
```python
import tomli_w

doc = {"one": 1, "two": 2, "pi": 3}
with open("path_to_file/conf.toml", "wb") as f:
    tomli_w.dump(doc, f)
```
2. write to string
```python
import tomli_w

doc = {"table": {"nested": {}, "val3": 3}, "val2": 2, "val1": 1}
expected_toml = """\
val2 = 2
val1 = 1

[table]
val3 = 3

[table.nested]
"""
assert tomli_w.dumps(doc) == expected_toml
```

3. write with multi-line
```python
import tomli_w

doc{"s": "here's a newline\r\n"}
expected_toml = '''\
s = """
here's a newline
"""
'''
assert tomli_w.dumps(doc, multiline_strings=True) == expected_toml
```
### tomlkit
- 서드 파티 솔루션
- toml 읽기쓰기가 모두 가능한 라이브러리
- 대부분의 경우 파이썬에서 toml을 지원하는 가장 적합한 솔루션

```bash
pip install tomlkit
```

1. Parsing
```python
from tomlkit import dumps
from tomlkit import parse # also use loads

content = """[table]
foo = "bar" # String
"""
doc = parse(content)

# doc is a TOMLDocument instance that holds all the information
# about the TOML string.
# It behaves like a standard dictionary.

assert doc["table"]["foo"] == "bar"

# The string generated from the document is exactly the same
# as the original string

assert dumps(doc) == content
```

2. Modifying
```python
from tomlkit import dumps
from tomlkit import parse
from tomlkit import table

doc = parse("""[table]
foo = "bar" # String
""")

doc["table"]["baz"] = 13

dumps(doc)

# Add a new table
tab = table()
tab.add("array", [1, 2, 3])

doc["table2"] = tab

dumps(doc)

# Remove the newly added table
doc.pop("table2")
# del doc["table2"] is also possible
```

3. Writing
- Create a TOML file with this code
```python
from tomlkit import comment
from tomlkit import document
from tomlkit import nl
from tomlkit import table

doc = document()
doc.add(comment("This is a TOML document."))
doc.add(nl())
doc.add("title", "TOML Example")
# Using doc["title"] = "TOML Example" is also possible

owner = table()
owner.add("name", "Tom Prestom-Werner")
owner.add("organization", "GitHub")
owner.add("bio", "GitHub Cofounder & CEO\nLikes tater tots beer.")
owner.add("dob", datetime(1979, 5, 27, 7, 32, tzinfo=utc))
owner["dob"].comment("First class dates? Why not?")

# Adding the table to the document
doc.add("owner", owner)

database = table()
database["server"] = "192.168.1.1"
database["ports"] = [8001, 8001, 8002]
database["connection_max"] = 5000
database["enabled"] = True

doc["database"] = database
```
- Created result TOML file
```toml
# This is a TOML document.

title = "TOML Example"

[owner]
name = "Tom Preston-Werner"
organization = "GitHub"
bio = "GitHub Cofounder & CEO\nLikes tater tots and beer."
dob = 1979-05-27T07:32:00Z # First class dates? Why not?

[database]
serveer = "192.168.1.1"
ports = [8001, 8001, 8002]
connection_max = 5000
enabled = true
```

## `pyproject.toml`
- pip에서 패키지 빌드 시 사용하는 빌드 시스템 요구 사항 및 정보
- 최신 파이썬 패키지는 `pyproject.toml` 파일을 포함할 수 있음

### Build process
> 패키지 구축을 위한 전체 프로세스

    1. 격리된 빌드 환경 생성
    2. 빌드 종속성으로 빌드 환경 조성
    3. 필요, 가능한 경우 패키지 메타데이터 생성
    4. 패키지 휠 생성

#### 1. Build isolation
- pip에서 빌드 명령에 대해 sys.path에 추가된 임시 디렉토리에 빌드 시간 Python 종속성 설치
- 빌드 요구 사항이 사용자의 런타임 환경과 독립적으로 처리됨

#### 2. Build time dependencies
- `pyproject.toml`파일 내부의 `build-system.requires` 키
    - 패키지의 빌드 시간 종속성에 대한 요구 사항 지정자 목록
```toml
[build-system]
requires = ["setuptools ~= 58.0", "cython ~= 0.29.0"]
```
- 빌드 백엔드에서 get_requires_for_build_wheel 후크를 사용하여 동적으로 계산된 빌드 종속성 제공 지원
- 빌드 시간 요구 사항 지정자는 URL 포함 패키지 참조 지원
```toml
[build-system]
requires = ["setuptools @ git+https://gihub.com/pypa/setuptools.git@main"]
```

#### 3. Metadata generation
    prepare_metadata_for_build_wheel

#### 4. Wheel generation

#### 5. Editable installation
    build_wheel_for_editable

#### 6. Backend configuration
`--config-settings` 명령줄 옵션

### Build output
빌드 백엔드에서 출력의 인코딩 유효성 검증

### Fallback behaviour
- `pyproject.toml`에 빌드시스템섹션이 없는 경우:
```toml
[build-system]
requires = ["setuptools>=40.8.0"]
build-backend = "setuptools.build_meta:__legacy__"
```
- 빌드 시스템 섹션은 있지만 빌드 백엔드가 없는 경우:
    - `setuptools` 포함 전제
    - `setuptools.build_meta:__legacy__` 빌드 백엔드 사용

### Disabling build isolation
- `--no-build-isolation` 플래그를 사용해 pip의 빌드 시간 종속성 관리 비활성화 가능
    - 개발자 수동 관리
