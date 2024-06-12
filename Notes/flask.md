---
tags:
  - PYTHON
---
# Flask
## Tutorial

    💫 튜토리얼 프로젝트에 앞서...

- Python 기본적인 사용이 익숙함을 전제
- 기본적인 블로그 애플리케이션(called as *Flaskr*) 만들기
- 앱 기능
    - 회원 등록 및 로그인
    - 게시글 생성, 수정, 삭제
- 완성한 애플리케이션 패키징 및 배포 후 설치까지


<span style="color: pink;">***Flask is flexible!***</span>

[Tutorial](https://flask.palletsprojects.com/en/3.0.x/tutorial/factory/)

### 1. Project Layout
- `hello.py`

basic flask app

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'
```

### 2. Application Setup
#### Application Factory
```bash
mkdir flaskr
```
- `flaskr/__init__.py`

```bash
flask --app flaskr run --debug
```

### 3. Define and Access the Database
- `flaskr/db.py`
- `flaskr/schema.sql`

```bash
flask --app flaskr init-db
```

### 4. Blueprints and Views
#### Create a Blueprint
- `flaskr/auth.py`

### 5. Templates
- `flaskr/templates/base.html`
- `flaskr/templates/auth/register.html`
- `flaskr/templates/auth/login.html`

### 6. Static Files
- `flaskr/static/style.css`

### 7. Blog Blueprint
- `flaskr/blog.py`
- `flaskr/templates/blog/create.html`
- `flaskr/templates/blog/index.html`
- `flaskr/templates/blog/update.html`

### 8. Make the Project Installable
#### Describe the Project
- `pyproject.toml`

#### Install the Project
```bash
pip install -e .
```
- confirm
```bash
pip list
```
```
Package                      Version              Editable project location
---------------------------- -------------------- ----------------------------------
attrs                        23.2.0
...
Flask                        3.0.3
★ flaskr ★                   1.0.0                /home/jhi/workspace/flask-tutorial
furl                         2.1.3
...
```

### 9. Test Coverage
- 애플리케이션에 대한 단위 테스트
- Flask 테스트 클라이언트(test client)
    - 애플리케이션에 대한 요청을 시뮬레이션하고 응답 데이터를 반환
```bash
pip install pytest coverage
```
- `tests/data.sql`
```sql
INSERT INTO user (username, password)
VALUES
  ('test', 'pbkdf2:sha256:50000$TCI4GzcX$0de171a4f4dac32e3364c7ddc7c14f3e2fa61f2d17574483f7ffbb431b4acb2f'),
  ('other', 
  'pbkdf2:sha256:50000$kJPKsz6N$d2d4784f1b030a9761f5ccaeeaca413f27f2ecb76d6168407af962ddce849f79');
INSERT INTO post (title, body, author_id, created)
VALUES
  ('test title', 'test'||x'0a'||'body', 1, '2018-01-01 00:00:00');
```

### 10. Deploy to Production
### 11. Keep Developing!
