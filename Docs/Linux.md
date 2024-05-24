---
tags:
  - Linux
---
# Linux
➰　Focusing on Command

### 📌 기본 개념 정리

#### ⚡ shell
- 명령어 처리기
- sh, bash, zbash, ksh, csh 등

#### ⚡ shell script
- `shell` 을 사용해 실행할 명령을 텍스트로 작성하여 저장한 프로그램
- `sciript`
    - interpreter 방식으로 동작하는, 컴파일 되지 않는 프로그램

#### ⚡ sh
- 프롬프트 `$`
- *Bourne shell* 의 줄임말로 `sh`라고 부름

#### ⚡ bash
- 프롬프트 `#`
- *Bourne-agin shell* (줄여서 `bash`)
- sh 와 대부분 호환됨
- `bashrc`
    - bash가 참고할 사항을 정의해 놓은 파일

#### ⚡ zbash
- 프롬프트 `%`
- bash, ksh, tcsh 등 일부 기능을 포함하고 개선한 확장형 쉘
- *command prompt* 에서 **자동완성**을 지원

### 📌 자주 사용하는 명령어
#### ⚡ Linux에서 listen 중인 Port 확인
```
$ sudo ss -lntu
```
#### ⚡ Buffer/Cache Memory 확인
```
$ free -m
```
#### ⚡ Buffer/Cache Memory 삭제
```
$ sudo sysctl -w vm.drop_caches=3
```
#### ⚡ Directory 접근 권한 부여
```
$ sudo chown -R {user} {directory}

$ sudo chown -R $USER $(pwd)
```
