---
tags:
  - DEVELOP
  - CMD
---
# Command
## 🎇 Linux
- 리눅스 커맨드라인 `$`

| |Command|Option|Subject| |Annotation|
|:---:|:---|:---|:---|:---:|:---|
| |**pwd**||현재 디렉토리 절대경로 표시| |`$ pwd`|
|⭐|**clear**||터미널 화면 초기화| |`$ clear`|
| |**ls**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= list</span>|현재 디렉토리 파일 목록 {*option*} 표시| |`$ ls`|
|⭐||*-a*|+ 숨김 파일까지| |`$ ls -a`|
|⭐||*-l*|+ 목록을 자세히| |`$ ls -l`|
| ||*-d*|+ 디렉토리 목록을| |`$ ls -d`|
| ||**.txt*|+ 확장자가 txt인 파일만| |`$ ls *.txt`|
| ||*a**|+ 파일명이 'a'로 시작하는 파일만| |`$ ls a*`|
| ||*{dir_name}*|+ 해당 디렉토리 하위 파일만| |`$ ls dname`|
| |**cd**||디렉토리 {*option*} 이동| |`$ cd`|
| ||*~user user*|+ 사용자의 홈 디렉토리로| |`$ cd ~user user`|
| ||*..*|+ 직전 상위 디렉토리로| |`$ cd ..`|
| ||*{abs_path}*|+ 절대경로 디렉토리로| |`$ cd D:/workspace`|
| ||*{rel_path}*|+ 상대경로 디렉토리로| |`$ cd ../Assets`|
| |**rm**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= remove</span>|{*option*} 삭제| |`$ rm target.file`|
| ||*-i*|+ 삭제 확인 메시지 출력 후| |`$ rm -i target.file`|
| ||*-f*|+ 확인 메시지 없이 즉시<span style="background: rgba(255,255,0,0.2);">= force</span>| |`$ rm -f target.file`|
| ||*-r*|+ 빈 디렉토리| |`$ rm -r dname`|
|⭐||*-rf*|+ 파일이 있는 디렉토리| |`$ rm -rf dname`|
| |**touch**||파일 {*option*} 생성(갱신)| |`$ touch fname`|
| ||*{file_name} {file_name} ...*|+ 여러 개 동시| |`$ touch aname bname`|
| |**cp**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= copy</span>|{*option*} 복사| |`$ cp origin copy`|
| ||*{file_name} {copy_name}*|+ 파일| |`$ cp forigin fcopy`|
| ||*-r {dir_name} {copy_name}*|+ 디렉토리| |`$ cp -r dorigin dcopy`|
| |**mv**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= move</span>|파일 {*option*} 이동(이름 혹은 위치 변경)| |`$ mv fpath tpath`|
| ||*{file_name} {file_name} ...*|+ 여러 개 동시| |`$ mv apath bpath tpath`|
| ||*{origin_name} {result_name}*|+ 이름| |`$ mv fname tname`|
| |**mkdir**||디렉토리 {*option*} 생성| |`$ mkdir ../dname`|
| ||*-p*|+ 상위 디렉토리 포함 자동| |`$ mkdir -p ./dname/dname`|
| |**rmdir**||빈 디렉토리 {*option*} 삭제| |`$ rmdir dname`|
| ||*-p*|+ 경로 모두| |`$ rmdir -p adir/bdir/cdir`|
| |**file**||파일 종류 표시| |`$ file target.file`|
| |**cat**||파일 내용 {*option*} 출력| |`$ cat fname`|
| ||*{file_name} {file_name} ...*|+ 연결해서| |`$ cat afile bfile cfile`|
| |**head**||파일 앞부분 10행 출력| |`$ head target.file`|
| ||*-n*|+ 출력할 행 수 지정| |`$ head -n 5`|
| |**tail**||파일 뒷부분 10행 출력| |`$ tail target.file`|
| ||*-n*|+ 출력할 행 수 지정| |`$ tail -n 5`|
| |**more**||파일 내용을 페이지 단위로 출력| |`$ more target.file`|
| ||*+n*|+ n 번째 행부터 출력| |`$ more +10 target.file`|
| |||*space* : 다음 페이지로| ||
| |||*B* : 이전 페이지로| ||
| |||*Q* : 출력 종료| ||
| |**less**||파일 내용을 페이지 단위로 출력| |`$ less target.file`|
| ||*+n*|+ n 번째 행부터 출력| |`$ less +10 target.file`|
| |||*space, →, page up* : 다음 페이지로| ||
| |||*B, ←, page down* : 이전 페이지로| ||
| |||*Q* : 출력 종료| ||
| |**whoami**||현재 접속 사용자 확인| |`$ whoami`|
| |**whatis**|*{command}*|명령어의 간략한 설명 확인| |`$ whatis pwd`|
| |**man**|*{command}*|명령어 매뉴얼 확인| |`$ man pwd`|
| |||*Q* : 출력 종료| ||
| |**alias**||사용자 명령어 확인| |`$ alias`|
| ||*{user_command}="{command_thing}"*|+ 사용자 명령어 등록| |`$ alias tcommand="pwd"`|
| |**unalias**|*{user_command}*|사용자 명령어 삭제| |`$ unalias tcommand`|
| |**python**||파이썬 {option} 명령어| |`$ python`|
|⭐||*{file_name}.py*|+ 파일 실행| |`$ python file.py`|
| |||*exit()* : 파이썬 모드 종료| ||
|⭐|**>**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">Redirection 연산자</span>|명령어 실행 결과를 다음 파일로 전달(→덮어쓰기 저장)| |`$ pyside6-uic uiform.ui > uiform.py`|
| |**>>**||명령어 실행 결과를 이어쓰기 저장| |`$ pyside6-uic uiform.ui >> uiform.py`|
| |**wc**||파일 정보 {*option*} 출력| |`$ wc target.file`|
| ||*-l*|+ 줄(line) 수만| |`$ wc -l target.file`|
| ||*-w*|+ 단어(word) 수만| |`$ wc -w target.file`|
| ||*-c*|+ 철자(char) 수만| |`$ wc -c target.file`|
|⭐|**grep**||파일에서 {*option*} 특정 단어나 구문 검색| |`$ grep keyword target.file`|
| ||*-i*|+ 대소문자 구분없이| |`$ grep -i keyword target.file`|
| |**\|**||명령어 연결(Pipe)| |`$ pip list \| grep wheel`|
| |**awk**|'*(condition)* {print $*(col_num)*}' *(file_name)*|파일에서 조건에 맞는 컬럼 출력| |`$ awk '$3 != "MAN" {print $2, $3}' person.txt >> notman.txt`|
| ||*$0*|+ 전체 컬럼| |`$ awk '$2 == "A" {print $0}' asset.txt > result.txt`|
| ||*$n=="VALUE"*|+ n번 컬럼의 값이 "VALUE"와 같은 경우| |`$ awk '$3 == "apple" {print $2, $3}' fruit.txt > apple.txt`|
| ||*substr($n, 시작, 끝)*|+ 지정문자열의 시작부터 끝까지| |`$ awk 'substr($2, 1, 1)=="A" {print $2, $6}' asset.txt`|
| |**sort**||데이터를 특정 컬럼 기준으로 {*option*} 정렬| |`$ sort 6 emp.txt`|
| ||*-k*|+ 오름차순| |`$ sort -k 6 emp.txt`|
| ||*-rk*|+ 내림차순| |`$ sort -rk 6 emp.txt`|
| ||*-n*|+ 숫자정렬옵션| |`$ sort -n 6 emp.txt`|
| |**uniq**||중복라인제거| |`$ awk '{print $3}' emp.txt \| sort -k 1 \| uniq`|
| |**echo**||변수 출력(print)| |`$ a=1 \| echo $a`|
| |**diff**||파일 간 차이점 검색| |`$ diff afile bfile`|
| |**find**||파일 {*option*} 검색| |`$ find /root -name 'target.file' -print`|
| ||*-maxdepth 0*|+ 디렉토리 검색 깊이 지정| |`$ find /root -maxdepth 1 -name 'target.file' -print`|
|⭐|**tar**||파일 {*option*} 압축| |`$ tar -zcvf empAll.tar.gz emp*.txt`|
| |||파일 {*option*} 압축 해제| |`$ tar -zxvf empAll.tar.gz`|
| ||*-z*|+ 실제 파일 크기를 압축| ||
| ||*-c*|+ 여러 개의 파일을 하나로| ||
| ||*-v*|+ 작업 과정을 출력| ||
| ||*-f*|+ 생성 파일명 지정| ||
| ||*-x*|+ 압축 파일 해제| ||
| ||*-*|+ 압축 해제 위치 지정| ||
| |**sed**||파일 내용 검색 및 변경 출력| |`$ sed emp.txt`|
| ||'s/*(origin)*/*(result)*'|+ 기존 데이터를 지정 데이터로 변경해서 출력(원본 유지)| |`$ sed 's/KING/yyy' emp.txt >> emp2.txt`|
| |**su**||접속 사용자 변경(root default)| |`$ sudo su`|
|⭐|**chmod**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= Change owner</span>|파일, 디렉토리 권한 변경| ||
| |**chown**||파일, 디렉토리 소유자 변경| |`$ chown `|
| ||*(group name):(user name)*|+ 소유자를 특정그룹의 특정사용자로 변경| |`$ chown tgroup:tuser target.file`|
| ||*-R*|+ 디렉토리 하위 권한 일괄 변경| |`$ chown -R oracle:oracle test`|
|⭐|**chmod**|<span style="background: rgba(255,255,0,0.2); font-weight: 600;">= Change mode</span>|파일, 디렉토리 권한 변경| ||
| ||*u*|+ 사용자 권한| ||
| ||*g*|+ 그룹 권한| ||
| ||*o*|+ 기타 사용자 권한| ||
| ||*-rwx*|+ r: 읽기 / w: 쓰기 / x: 실행| |`$ chmod u-rwx,g-rwx,o-rwx skin.csv`<br>`$ chmod u+rwx,g+rw,o+x skin.csv`|
| ||*421*|+ 4: 읽기 / 2: 쓰기 / 1: 실행| |(읽기)`$ chmod 444 skin.csv`<br>(읽쓰)`$ chmod 666 skin.csv`<br>(읽쓰실)`$ chmod 777 skin.csv`|
| ||*-R*|+ 디렉토리 하위 권한 일괄 변경| |`$ chmod -R 777 dname`|
| |**chattr**||일반 사용자 권한 임의 변경 방지 설정(root 사용자만 가능)| ||
| ||*+i*|+ | |`(root) $ chattr +i target.file`|
| ||*-i*|+ | |`(root) $ chattr +i target.file`|
| |||| ||

## 🎇 CMD
- 윈도우 커맨드라인 `>`

| |Command|Option|Subject| |Annotation|
|:---:|:---|:---|:---|:---:|:---|
||**help**||명령줄 도움말| |`> help`|
|⭐|**ipconfig**||Window IP 구성 및 네트워크 설정 확인| |`> ipconfig`|
| ||*/all*|+ 상세 정보| |`> ipconfig /all`|
||**path**||환경변수 확인| |`> path`|
||**mode**||CON 장치 상태| |`> mode`|
| |**whoami**||현재 사용자 확인| |`> whoami`|
||**tree**||디렉토리 구조 그래픽 출력| |`> tree`|
||**tasklist**||현재 실행 중인 프로세스 목록| |`> tasklist`|
||**cls**||현재 명령 프롬프트 화면 초기화| |`> cls`|
||**:**||작업 드라이브 변경| |`> D:`|
||**cd**||현재 디렉토리 확인| |`> cd`|
| ||*{path}*|+ 디렉토리 이동| |`> cd ..`|
||**dir**||디렉토리 정보 및 하위 항목 확인| |`> dir`|
||**md**<br>**mkdir**|*{directory}*|디렉토리 생성| |`> md dname`|
||**rd**<br>**rmdir**|*{directory}*|디렉토리 삭제| |`> rd dname`|
| ||*/s*|+ 비어있지 않은 디렉토리 삭제| |`> rd /s dname`<br>`> rd dname /s`|
||**del**|*{file}*|파일 삭제(정규식 사용가능)| |`> del *.log`|
||**copy**|*{origin file} {output path}*|파일 복사| |`> copy a.txt D:\dname\`|
||**xcopy**|*{origin file} {output path}*|파일 및 디렉토리 숨김 파일까지 복사| |`> xcopy D:\*.* %userprofile%\desktop \S`|
||**move**|*{sourcefile} {destination}*|파일 이동| |`> move target.file ./dname`|
||**echo**|*{message}*|콘솔에 메시지를 출력| |`> echo 이 문장을 출력합니다.`|
||**type**|*{text.file}*|텍스트 파일의 내용을 콘솔에 출력| |`> type target.txt`|
||**rename**|*{file}*|파일, 디렉토리 이름 변경| |`> D:`|
||**date**||현재 날짜 보기 및 새로운 날짜 입력| |`> date`|
||**exit**||명령 프롬프트 창 종료| |`> exit`|
| |||| ||

## 🎇 Pip
| |command|option|subject| |
|:---:|:---|:---|:---|:---:|
| |**`python -m pip --version`**||설치 버전 확인| |
| |**`pip install`**|*{package}*|패키지 최신버전 설치| |
| ||*{package}=={version}*|패키지 특정버전 설치| |
| ||*{package}>={version}*|패키지 최소버전 지정 설치| |
| ||*-r requirements.txt*|의존성 패키지 설치| |
| ||*--upgrade {package}*|패키지 버전 업그레이드| |
| |**`pip uninstall`**|*{package}*|패키지 삭제| |
| ||*-r requirements.txt*|의존성 패키지 삭제| |
| |**`pip inspect`**||파이썬 환경 패키지 검사(→ json 반환)| |
| ||*--local*|+ 전역 설치된 패키지 제외| |
| ||*--user*|+ 현재 사용자의 설치 패키지만 검사| |
| ||*--path {path}*|+ 지정 경로 패키지 검사| |
| |**`pip list`**||설치된 패키지 목록 화인| |
| ||*--format=json*|패키지 목록을 json으로 출력| |
| ||*--format=freeze*|패키지 목록을 freeze 형식으로 출력| |
| ||*--outdated*|최신화 아닌 패키지만 출력| |
| ||*--not required*|의존성과 무관한 패키지만 출력| |
| |**`pip show`**|*{package}*|패키지 정보 확인| |
| ||*--verbose {package}*|패키지 전체 정보 확인| |
| |**`pip freeze`**||설치된 패키지 버전 목록 확인| |
| ||*> requirements.txt*|패키지 버전 목록 파일 저장| |
| |**`pip check`**||설치 패키지 종속성 검증| |
| |**`pip download`**|*{package}*|패키지 다운로드| |
| |||| |

## 🎇 conda
|⭐|Command|Option|Subject| |
|:---:|:---|:---|:---|:---:|
| |**`conda`**|*--version*|설치 버전 확인| |
| |**`conda info`**||설치 정보 확인| |
| |**`conda list`**||설치 패키지 목록 확인| |
| |**`conda env list`**||가상환경 목록 조회| |
| |**`conda create`**|*-n {env_name}*|가상환경 생성| |
| ||*-n {env_name} python={version}*|가상환경 파이썬 버전 지정| |
| ||*--clone {source_env_name} -n {env_name}*|가상환경 복제| |
| |**`conda env remove`**|*-n {env_name} --all*|가상환경 삭제| |
| |**`conda activate`**|*{env_name}*|지정 가상환경 활성화| |
| |**`conda deactivate`**||현재 가상환경 비활성화| |
| |**`conda install`**|*{package}*|현재 가상환경에 패키지 설치| |
| ||*-n {env_name} {package}*|특정 가상환경에 패키지 설치| |
| |**`conda update`**|*{package}*|설치된 패키지 업데이트| |
| |**`conda remove`**|*-n {env_name} {package}*|설치된 패키지 삭제| |
| |**`conda config`**|*--append {env_path} {destination}*|가상환경 설치 위치 변경| |
| |||| |

## 🎇 Docker
|⭐|Command|Option|Subject| |Annotation|
|:---:|:---|:---|:---|:---:|:---|
||****||{*option*}| |`$ `|
| ||*-*|+ | |`$ `|
||****||{*option*}| |`$ `|
| ||*-*|+ | |`$ `|
| |||| ||