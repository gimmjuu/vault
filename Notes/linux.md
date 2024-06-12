---
tags:
  - LINUX
---
# Linux
➰　Focusing on Command

## 🎇 shell
- 명령어 처리기
- sh, bash, zbash, ksh, csh 등

### 📌 shell script
- `shell` 을 사용해 실행할 명령을 텍스트로 작성하여 저장한 프로그램
- `sciript`
    - interpreter 방식으로 동작하는, 컴파일 되지 않는 프로그램

### 📌 sh
- 프롬프트 `$`
- *Bourne shell* 의 줄임말로 `sh`라고 부름

### 📌 bash
- 프롬프트 `#`
- *Bourne-agin shell* (줄여서 `bash`)
- sh 와 대부분 호환됨
- `bashrc`
    - bash가 참고할 사항을 정의해 놓은 파일

### 📌 zbash
- 프롬프트 `%`
- bash, ksh, tcsh 등 일부 기능을 포함하고 개선한 확장형 쉘
- *command prompt* 에서 **자동완성**을 지원

## 🎇 자주 사용하는 명령어
### 📌 Linux에서 listen 중인 Port 확인
```bash
sudo ss -lntu
```

### 📌 Buffer/Cache Memory 확인
```bash
free -m
```

### 📌 Buffer/Cache Memory 삭제
```bash
sudo sysctl -w vm.drop_caches=3
```

### 📌 Directory 접근 권한 부여
```bash
sudo chown -R {user} {directory}

sudo chown -R $USER $(pwd)
```

## 🎇 추가
### 📌 Linux Ubuntu 22.04에서 Python 가상환경 사용하기
```bash
usr@device-001-usr:~/workspace$ python3 -m venv .venv
usr@device-001-usr:~/workspace$ source .venv/bin/activate
```

### 📌 wget으로 URL 파일 다운로드하기
➰　`wget` `[옵션]` `[URL]`

1. **현재 디렉토리에 파일 다운로드**
```bash
wget https://example.com/file.zip
```
2. **파일명 지정하기**
```bash
wget -O output_file.zip https://example.com/file.zip
```
3. **저장 경로 지정하기**
```bash
wget -P /path/to/save https://example.com/file.zip
```

### 📌 Swap 메모리 늘리기
[Ubuntu 22.04 - Swap 메모리 늘리기](https://codechacha.com/ko/ubuntu-add-swap/#2-swap-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%81%AC%EA%B8%B0-%ED%99%95%EC%9D%B8)
