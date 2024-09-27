---
updated:
    2024-09-19
tags:
    - CERTIFICATE
---

<link rel="stylesheet" href="../Assets/style.css" />

# Network Administrator

- National Accreditation Certificate Level 2

## 공통

- 우선순위?
    + 에뮬레이터★★★ > 라우터★★ > 케이블★
- 일반적으로 크로스 케이블보다 다이렉트 케이블 출제 빈도가 더 높음
- 윈도우 에뮬레이터 서비스 문제는 항목이 많아서 틀리는 경우가 많음

## 라우터 에뮬레이터

<details>
  <summary>♧ 라우터 기본 명령어</summary>

|명령어|단축 명령어|동작|
|:-:|:-:|:-|
|<span class="_yellowbold">공통</span>|　|　|
|enable|en|관리자 설정 모드 진입|
|configure terminal|conf t|전역 설정 모드 진입|
|copy running-config startup-config|copy r s|현재 설정 저장|
|exit|-|이전 경로로 이동|
|end|-|관리자 설정 모드로 진입<br>(exit*2=end)|
|?|-|사용 가능한 명령어 조회|
|show ru|sh ru|세팅된 라우터 정보 조회|
|<span class="_yellowbold">인터페이스</span>|　|　|
|interface|[인터페이스명]|interface 진입|
||fastethernet 0/0|fastethernet 0/0 진입|
||serial 2/0|serial 2/0 진입|
|no shutdown|-|활성화 작업|
|ip add|[ip주소] [서브넷마스크]|ip/서브넷 마스크 설정|
|ip add|[ip주소] [서브넷마스크] secondary|두번째 ip/서브넷 마스크 설정|
|no ip add||ip/서브넷 마스크 초기화(모두 삭제)|
|bandwidth|[대역폭]|대역폭 지정|
|clock rate|[쿨럭 속도]|쿨럭 속도 지정(k=1000 단위)|
|description|[내용]|주석 설정|
|encapsulation frame-relay||frame relay 방식으로 캡슐화 설정|
|ip access-group [라우터번호] in|e.g. ip access-group 1 in|PC에서 Router1 들어가는 access-list 1 설정|
|<span class="_yellowbold">텔넷(telnet)/콘솔(console)</span>|　|　|
|line vty 0 4|virtual terminal|텔넷 설정(진입) 명령어|
|line console [콘솔번호]|e.g. line console 0|console 설정(진입) 명령어, default는 0|
|transport input ssh||ssh 보안 설정|
|transport input all||ssh 설정, 일반 텔넷 모두 설정|
|password|[패스워드]|패스워드 설정|
|no login||로그인 X|
|login||로그인 O|
|login local||개별 로그인|
|exec-timeout [분] [초]|e.g. exec-timeout 03 50 (3분 50초)|텔넷에서 입력이 없으면 세션 자동 종료|
|<span class="_yellowbold">설정</span>|　|전역 설정 모드 `Router(config)#` 에서 확인|
|snmp-server community [community명]|[권한 옵션]|community 모니터링 설정|
||ro|read only: 읽기만 가능, 권한 옵션 기본값|
||rw|read write: 읽고 쓰기 가능|
|username|[유저명] [패스워드]|계정 이름, 비밀번호 작성 후 계정 생성|
|ip default-gateway [ip]||기본 게이트웨이 ip 설정|
|ip default-network [ip]||기본 네트워크 ip 설정|
|ip domain-name [도메인명]||도메인명 설정|
|hostname [네트워크명]||호스트네임을 [네트워크명]으로 변경|
|ip dhcp pool [서버이름]||dhcp 서버 이름 설정|
|network [ip] [서브넷마스크]||dhcp 네트워크 ip 설정|
|ip excluded-address [ip]||dhcp 제외 ip 설정|
|router ospf 1||ospf 설정|
|network [네트워크 ID] [와일드카드마스크(서브넷마스크 반대)] area [area ID]|e.g.|network 정보 설정|
||network 192.70.100.0 0.0.0.255 area 1||
||network 193.150.60.0 0.0.0.255 area 1||
|<span class="_yellowbold">확인</span>|　|관리자모드 `Router#` 에서 확인|
|show interface||인터페이스 정보 확인|
|show user||사용자 정보 확인|
|show ip route||라우팅 테이블 정보 확인|
|show flash||플래시 내용을 확인|
|show process||프로세스 정보 확인|
|show host||host 정보 확인|
|show version||소프트웨어 버전, IOS, 부트로더 버전 확인|

</details>

### 라우터 IP/서브넷 마스크 설정

```
q/ ROUTER 1의 FastEthernet 0/0의 IP를 192.168.0.100/24로 설정하시오.
    (완료된 설정은 startup-config에 저장하시오.)
```

- 문제의 ROUTER 1/2 구분 ★
- (*기본 세팅*) 명령 프롬프트 창에서
    1. en (enable): 사용자 모드에서 관리자 모드로 전환
    2. conf t (configure terminal): 관리자 모드에서 전역 설정 모드로 전환
3. interface fastethernet 0/0: 문제의 fastethernet 0/0으로 이동
4. ip add 192.168.0.100 255.255.255.0: IP/서브넷 마스크 설정 (*필수암기*)
5. /24 : 2진수를 앞에서부터 채워서 계산한다
    + 2진수 11111111.11111111.11111111.00000000
    + = 10진수 255.255.255.0
6. no shutdown : 활성화
    + 활성화를 하라는 문제도 있고 아닌 문제도 있음 ★
7. exit : fastethernet 에서 나가기
8. exit : 전역 모드에서 나가기
9. copy r s (copy running-config startup-config)
```sh
# 문제의 ROUTER 1/2 선택 구분 ★
# 기본 세팅 --------------------------------------------------------
en  # (enable): 사용자 모드에서 관리자 모드로 전환
conf t  # (configure terminal): 관리자 모드에서 전역 설정 모드로 전환
# ------------------------------------------------------------------
interface fastethernet 0/0  # 문제의 fastethernet 0/0으로 이동
ip add 192.168.0.100 255.255.255.0  # IP/서브넷 마스크 설정
no shutdown # 활성화
exit # fastethernet 에서 나가기
exit # 전역 모드에서 나가기
# 공통 저장 --------------------------------------------------------
copy r s # (copy running-config startup-config)
```

### 라우터 대역폭 설정

> 저장 명령어는 모든 문제가 동일함 → 설정 명령어 부분 집중 숙지

```
q-1/ ROUTER2의 Serial 2/0의 대역폭을 2048로 설정하시오.
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 stratup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 2에서
en
conf t
interface serial 2/0  # serial 2/0 설정 인터페이스 모드
bandwidth 2048  # 대역폭을 2048로 변경
exit  # serial 2/0에서 나가기
exit  # 전역 설정 모드에서 나가기
copy r s  # 저장 명령어, 모든 라우터 문제의 끝
```

```
q-2/ ROUTER1의 Serial 2/0의 클럭 속도를 72K로 설정하시오.
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 1에서
en  # 관리자 모드로
conf t  # 전역 설정 모드로
interface serial 2/0
clock rate 72000  # 클럭 속도를 72k로 설정 (k=1000)
exit  # serial 2/0 모드 나가기
exit  # conf t 모드 나가기
copy r s
```

### 라우터 주석 설정

```
q/ FastEthernet 0/0의 Description을 설정하시오.
    Description : ICQA
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER
en  # enable
conf t  # configure terminal
interface fastethernet 0/0
description ICQA  # 대소문자 구분 ○
exit    # 전역설정 모드로
exit    # 관리자 모드로
copy r s    # 설정 파일 저장
```

### 라우터 Secondary IP Address 설정

```
q/ ROUTER1의 Serial 2/0을 사용가능하게 IP 주소를 192.168.0.101/24와 두번째 IP 192.168.0.102/24로 설정하고 활성화 하시오.
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 1 선택
en
conf t
interface serial 2/0
ip add 192.168.0.101 255.255.255.0  # 24비트를 8비트씩 끊어서 2진수로 표현
ip add 192.168.0.102 255.255.255.0 secondary    # 두번째 IP 주소 설정
no shutdown # 활성화
exit    # 인터페이스 → 전역 설정
exit    # 전역 설정 → 관리자
copy r s
# Enter
```

### 라우터 기본 게이트웨이

```
q-1/ 기본 게이트웨이를 설정하시오. IP: 192.168.0.10
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER
en
conf t
ip default-gateway 192.168.0.1  # 기본 게이트웨이 IP 설정 명령어
exit    # 전역 설정 모드 → 관리자 모드
copy r s
```

```
q-2/ ROUTER1의 DHCP 네트워크를 192.168.100.0/24 서버 이름은 'icqa'로 설정하시오. 
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 1 선택
en
conf t
ip dhcp pool icqa 192.168.100.0 255.255.255.0   # dhcp 서버 이름 전역 설정
network 192.168.100.0 255.255.255.0 # dhcp 서버 IP 주소 전역 설정
# 서버 이름을 먼저 설정하고 IP 주소를 설정해야 함 ★
exit    # 전역 설정 모드로
exit    # 관리자 모드로
copy r s
```

### 라우터 텔넷 패스워크 설정

```
q/ ROUTER1 Telnet에 접근하는 Password를 icqa로 설정하고 로그인 하시오.
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 1 선택
en
conf t
line vty 0 4    # 텔넷 설정 명령어 (가상 터미널 0~4까지 총 5개 사용을 의미)
password icqa   # 텔넷 비밀번호 설정 (대소문자 구분)
login   # 텔넷 로그인 → 문제에 따라 로그인을 요구하지 않는 경우도 있음
exit
exit
copy r s
```

### 라우터 텔넷 세션 자동종료 설정

```
q/ 텔넷 연결 후 3분 50초 동안 입력이 없으면 세션이 자동 종료 되도록 설정하시오.
    (완료된 설정은 'Router#copy running-config startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용금지)
```
```sh
# ROUTER 선택
en  # 관리자 모드 접속
conf t  # 전역 설정 모드 접속
line vty 0 4    # 텔넷 연결
exec-timeout 03 50  # 3분 50초 후 세션 자동 종료
exit
exit
copy r s
```

### 라우터 콘솔 패스워드 설정

```
q/ ROUTER1 console 0의 패스워드를 ICQA로 설정하고 로그인하시오.
    (완료된 설정은 ...)
```
```sh
# ROUTER 1 선택
en
conf t
line console 0  # console 접속은 line
password ICQA
login   # 확인 필수!
exit
exit
copy r s
```

### 라우터 활성화 설정

```
q/ ROUTER 1 Serial 2/0을 활성화 시키시오.
```
```sh
# ROUTER 1 선택
en
conf t
interface serial 2/0
no shutdown
exit
exit
copy r s
```

### 라우터 호스트 네임 설정

```
q/ Hostname을 network2로 변경하고 console 0의 password를 route5로 변경한 후 로그인 하시오.
```
```sh
# ROUTER 선택
en
conf t
hostname network2   # 호스트네임 설정 (대소문자 구분)
line console 0  # 콘솔 모드 접속
password route5 # 패스워드 설정
login   # 로그인
exit
exit
copy r s
```

### 라우터 확인문제 유형 분석

```
q/ 인터페이스 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en  # 설정을 변경해야 하는 문제가 아니면 전역모드로 들어갈 필요 없음!
show interface  # show ~ : 정보 확인
copy r s
```
```
q/ 접속한 사용자 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show user
copy r s
```
```
q/ 라우팅 테이블 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en conf t
show ip route
copy r s
```
```
q/ 플래쉬 내용을 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show flash
copy r s
```
```
q/ 프로세스 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show process
copy r s
```

## UTP 케이블 제작

- RJ-45 커넥터
    + 흔히 랜선에서 PC나 공유기에 꽂는 플라스틱 소재의 헤더 파트
- 피복탈피기
    + UTP 케이블의 피복을 벗겨주는 툴 ← 시험장에 들고 가야 함!
- UTP 케이블
- 랜툴
    + UTP 케이블에 RJ-45 커넥터를 고정하는 툴 ← 시험장에 들고 가야 함!
- 케이블 테스터기
    + UTP 케이블 제작 후 정상적으로 케이블을 만들었는지 확인하는 기기
- 제작 방법
    1. 피복 탈피기로 UTP 양쪽 케이블의 피복을 1~2 바퀴 돌려서 벗겨낸다.
    2. 꼬여있는 선을 풀어서 케이블 타입에 맞게 색깔 순서대로 배열한다.
        + 다이렉트 케이블 기준 *주띠-주-녹띠-파-파띠-녹-갈띠-갈*
    3. 배열한 선은 랜툴이나 니퍼로 선끝을 잘라 정리한다.
        + 가지런히 정리해야 커넥터에 끼울 때 꼬이는 일이 없음
    4. 배열한 선을 RJ-45 커넥터에 끼워 랜툴로 찝어준다.
        + RJ-45는 평평한 면이 위로
        + 배열한 선이 RJ-45 밖으로 나오지 않게
    5. 반대쪽도 동일하게 진행.
        + 크로스 케이블의 경우 반대쪽 케이블 배열이 달라짐!
        + *녹띠-녹-주띠-파-파띠-주-갈띠-갈*
    6. 테스터기에 꽂아서 정상 연결을 확인한다.
        + 다이렉트 케이블 결과: 12345678 - 12345678
        + 크로스 케이블 결과:   12345678 - 36145278

## 윈도우 에뮬레이터

### 윈도우 서버 TCP/IP 설정

```
q/ 네트워크 환경에 <제시 문제>와 같이 IP 주소와 서브넷 마스크를 추가하시오.
    - IP Address:
        10101100.00010000.10010110.01110011
    - Subnet Mask:
        22bit
<추가 설정사항>
    - Default Gateway: 192.168.100.254
    - DNS Servers: 200.100.100.200
    - 추가 Gateway: 192.168.100.253
    - 보조 DNS Servers: 201.100.100.201
```
```
0. 윈도우 에뮬레이터 실행
1. [컴퓨터] - [네트워크 속성]
2. [internet Protocol Version 4] - [속성]
3. IP Address 계산
    + 10101100 = 172
    + 00010000 = 16
    + 10010110 = 150
    + 01110011 = 115
4. 서브넷 마스크
    + 11111111.11111111.11111100.00000000
    + = 255.255.252.0
```

### 윈도우 서버 DNS 설정

```
q/ 관리도구에 <제시 문제>와 같이 DNS 서버를 설정하시오.
@IN SOA ns.icqa.or.kr admin.icqa.or.kr(
    10 ; Serial
    15분 ; Refresh
    10분 ; Retry
    1일 ; Expire
    1시간); Minimum

- www IN A 192.168.100.20
ftp IN CNAME www
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [DNS]
2. [정방향 조회 영역] - [마우스우클릭] - [새 영역]
3. [다음>] - [주 영역] - [다음>]
4. [영역 이름] icqa.or.kr
5. [다음>] - [다음>] - [다음>] - [마침]
6. [icqa.or.kr] - [마우스우클릭] - [속성]
7. [SOA(권한 시작)] 입력 - [확인]
    + 일련번호: Serial
    + 주 서버: ns.icqa.or.kr
    + 책임자: admin.icqa.or.kr
8. [icqa.or.kr] - [마우스우클릭] - [새 호스트]
    + 이름: www
    + IP 주소: 192.168.100.20
9. [icqa.or.kr] - [마우스우클릭] - [새 별칭]
    + 별칭 이름: ftp
    + 대상 호스트: www
```

### 윈도우 서버 DHCP 설정

```
q/ 아래의 <보기>와 같이 IP를 할당할 수 있는 DHCP를 설정하시오.
1. 범위 이름: 네트워크관리사
2. 설명: 한국정보통신자격협회
3. 분배할 주소 범위: 192.168.106.1 ~ 254
4. 제외할 주소 범위: 192.168.106.2 ~ 25
5. 임대기간: 8일
6. 서브넷 마스크: 255.255.255.0(24bit)
7. 게이트웨이 설정: 192.168.106.1
8. 예약 설정
    - IP: 192.168.106.200
    - 예약 이름: Example
    - MAC주소: ff-ff-ff-ff-ff-ff
    - 설명: 예약
모두 설정한 뒤 활성화 하시오.
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [DHCP]
2. [IPv4] - [마우스우클릭] - [새 범위]
    + 문제 기재된 항목 이외의 페이지는 입력하지 않고 넘겨줌
3. [범위] - [예약] - [새 예약]
4. [범위] - [마우스우클릭] - [활성화]
```

### 윈도우 서버 IIS FTP 서버 설정

```
q/ 아래의 <보기>와 같이 FTP 사이트를 추가 및 설정하시오.
1. FTP 사이트 이름: ICQA
2. 실제경로: C:\inetpub\ftproot
3. IP주소: 192.168.100.10, 포틔 2121
4. 엑세스 허용: 익명 사용자(읽기 권한만)
5. 엑세스 거부 IP주소: 200.115.100.0/24
6. 최대 연결 수 메시지: 최대 접속 인원수를 초과하였습니다.
```
```
0. 윈도우 에뮬레이터 실행
1. [인터넷 정보 서비스 관리자]
2. [사이트] [마우스우클릭] [FTP 사이트 추가]
3. [icqa] - [FTP IPv4 주소 및 도메인 제한] - [저장]
    + 24비트: 11111111.11111111.11111111.00000000
    + 255.255.255.0
4. [icqa] - [FTP 메시지]
```

### 윈도우 서버 IIS 웹사이트 추가 및 설정

```
q/ 아래의 [보기]와 같이 Web 사이트를 추가 설정하시오.
1. 웹사이트 이름: ICQA
2. 실제 경로: C:\inetpub\ftproot
3. 웹사이트IP 주소: 192.168.100.10
4. 포트: 80
5. 호스트 이름: http://www.icqa.or.kr
6. 엑세스 허용 IP 주소: 192.168.100.0/24
```
```
0. 윈도우 에뮬레이터 실행
1. [인터넷 정보 서비스 관리자]
2. 
3. [IP 주소 및 도메인 제한] - [허용 항목 추가]
    + 서브넷 마스크가 없으면, [특정 IP 주소]
    + 서브넷 마스크가 있으면, [IP 주소 범위]
        * 192.168.100.0
        * 255.255.255.0
```

### 윈도우 서버 계정 추가

```
q/ 아래의 <보기>와 같이 계정을 추가 설정하시오.
1. ID: ICQA
2. Password: ICQAPass
3. 전체 이름: 전체관리자
4. 설명: 한국정보통신자격협회
5. 암호 변경할 수 없음
6. 암호 사용 기간 제한 없음
7. 소속그룹: Administrators
8. 로컬경로: C:\icqa
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [로컬 사용자 및 그룹]
2. [사용자] - [마우스우클릭] - [새 사용자]
3. [ICQA] - [마우스우클릭] - [속성]
4. [소속 그룹] - [추가] - [지금 찾기] - [선택 및 확인]
5. [프로필] - [홈 폴더] - [로컬 경로]
```

### 윈도우 서버 로컬보안정책 설정

```
q/ 아래의 <보기>와 같이 로컬보안정책을 설정하시오.
1. 패스워드는 최소 10일에서 최대 20일 사용
2. 패스워드는 3번 로그인 실패 시 30분간 계정 잠금
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [로컬 보안 정책]
2. [암호 정책]
    + 최대 암호 사용 기간
    + 최소 암호 사용 기간
3. [계정 잠금 정책]
```

### 윈도우 서버 서비스 설정

```
q/ 아래의 <보기>와 같이 서비스를 설정하시오.
원격 사용자가 Telnet을 이용하여 파일을 삭제하여 왔으나 정책이 변경되어 원격 사용자가 더 이상 로그온 할 필요가 없어졌다. 해당 기능을 중지시키고, 다시 시작할 수 없게 설정하시오.
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [서비스 관리자]
2. [Telnet]
    + 사용안함
    + 중지
```

## 단답형/선택형

### 2022년 3회

    인터넷 상에 전용회선과 같이 이용이 가능한 가상의 전용회선을 만들어 데이터를 도청하는 등의 행위를 방지하기 위한 통신규약이다.
    AH와 ESP 등 2개의 프로토콜로 구성되어 있다.

IPSec

    IP 주소의 낭비를 막기 위해 모든 호스트에 공인 IP 주소를 설정하고 내부적으로 사설 IP를 설정하며 인터넷에 접속할 때에만 공인 IP로 변환하는 기술은 무엇인가?

nat

    리눅스 시스템의 CPU 사용량, 우선순위 등을 실시간으로 전반적인 상황을 모니터링하거나 프로세스 관리를 할 수 있는 유틸리티이다.

top

    RAID의 구성에서 미러링 모드 구성이라고도 하며 디스크에 있는 모든 데이터는 동시에 다른 디스크에도 백업되어 하나의 디스크가 손상되어도 다른 디스크의 데이터를 사용할 수 있게 한 RAID 구성은?

RAID 1

    IPv6의 특징으로 옳은 것을 모두 고르시오.
        보기1. 64비트로 이루어져 있다.
        보기2. 확장 헤더 옵션이 있다.
        보기3. 유니캐스트, 멀티캐스트, 애니캐스트
        보기4. 유니캐스트, 멀티캐스트, 브로드캐스트
        보기5. 이동성이 좋아졌다.
        보기6. 보안성이 좋아졌다.

2,3,5,6

    네트워크 ID를 구하시오.
        IP: 170.10.50.2
        서브넷 IP: 255.255.224.0

170.10.32.0

<details><summary>[해설]</summary>

170이므로 B 클래스임을 확인  
255.255.224이므로 서브넷 개수는 8개  
서브넷 당 호스트 수는 32개이다.  
- 첫 번째 범위 : 170.10.0.0 ~ 170.10.31.255  
- 두 번째 범위 : 170.10.32.0 ~ 170.10.63.255  
170.10.50.2에서 50이 두 번째 범위에 속하므로 첫 번째 IP인 170.10.32.0이 정답이다.
</details>

### 2022년 2회

    대표적인 링크 상태 라우팅 프로토콜로 라우터 자신을 네트워크의 중심에 두고 최단 경로를 도출해 내는 프로토콜은?

OSPF

    Ping을 정상적으로 사용할 수 있도록 (A)를 채우시오.
        #echo (A) > /proc/sys/net/icmp_echo_ignore_all

(A) 0

<details><summary>[해설]</summary>

ignore_all을 0으로 설정해야 ping을 허용함
</details>

    라우팅 테이블, 네트워크 인터페이스 또는 네트워크 연결을 보여주는 리눅스 명령어는 무엇인가?

netstat

    IPSEC의 프로토콜 구조
        (A) IP 패킷에 대한 인증을 제공하고 데이터의 무결성을 보장하는 프로토콜 헤더
        (B) IP 패킷에 대한 인증과 암호화를 실시하고 데이터의 무결성과 기밀성을 보장하는 프로토콜 헤더
        (C) IP Sec 서비스를 구현할 때 암호화 및 인증에 사용할 요소를 정의하는 것

(A) AH (B) ESP (C) SA

    네트워크 계층에 포함되는 프로토콜 종류를 3가지 적으시오.

ICMP, IGMP, ARP, RARP

### 2022년 1회

    Distance Vector를 사용하며 최대 지원 가능한 홉 수가 15개이고 업데이트 주기는 30초인 라우팅 프로토콜은 무엇인가?

RIP

    TCP/IP 계층에서 전송 계층에 속하는 프로토콜을 두가지 적으시오.

TCP, UDP

    패스워드를 설정하는 리눅스 명령어는 무엇인가?

passwd

    웹서비스와 주고받는 HTTP 트래픽을 필더링, 모니터링 및 차단하는 특정 형태의 애플리케이션 방화벽은 무엇인가?

웹 방화벽(WAF)

    ICMP에 관한 설명

|Type|Message|
|:-:|:-|
|0|Echo Reply - 에코 응답|
|3|Destination Unreachable - 목적지 도달 불가능|
|4|Source Quench - 발신 제한|
|5|Redirect - 라우트 변경|
|8|Echo Request - 에코 요청|
|11|Time Exceeded - 시간 초과|

    A, B, C 각각의 설명에 맞는 것을 적으시오.
        (A) GNU 하에 개발된 리눅스 부트로더
        (B) 리눅스 커널 부팅이 완료된 뒤 실행되는 첫번째 프로세스. 커널이 직접 실행하는 유일한 프로세스.
        (C) 부팅 시 자동으로 마운트 되도록 설정해야 하는 파일.

(A) grub (B) init (C) etc/fstab

### 2021년 4회

    A와 B 각각의 설명에 맞는 프로토콜을 적으시오.
        (A) Distance Vector를 사용하며 최대 지원 가능한 홉 수가 15개이고 업데이트 주기는 30초인 라우팅 프로토콜
        (B) Link State를 사용하며 라우터 자신을 네트워크 중심에 두고 최단 경로를 도출해 내는 라우팅 프로토콜

(A) RIP (B) OSPF

    기본적으로 패스워드를 부여하거나 패스워드를 변경하는 리눅스 명령어

passwd

    스위칭이라는 LAN 기술을 기반으로 가상이라는 개념을 도입하여 네트워크 구성에 대한 지리적 제한을 최소화하면서 사용자가 원하는 최대한의 논리적인 네트워크를 구성할 수 있도록 수단을 제공하는 기술은 무엇인가?

VLAN

    TCP 특징으로 알맞은 것은?

연결성, 송수신 동일, 신뢰성

    드래그 & 드롭 문제 유형
        (A) 사설 IP를 공인 IP로 변경하는데 필요한 주소 변환 서비스
        (B) 네트워크에 연결된 장치에 IP 주소를 자동으로 할당하기 위해 사용되는 네트워크 관리 프로토콜
        (C) 네트워크 트래픽을 모니터링하고 제어하는 네트워크 보안 시스템
        (D) 인터넷망과 같은 공중망을 사용하여 둘 이상의 네트워크를 안전하게 연결하기 위해서 가상의 터널을 만든 후 데이터를 전송할 수 있는 네트워크이다.

(A) NAT (B) DHCP (C) Firewall (D) VPN

### 2021년 3회

    (A) 브라우저 사이에 전송되는 데이터를 암호화하여 인터넷 연결을 보호하기 위한 기술
    (B) 인증서를 기반으로 암호화된 데이터를 전송하는 프로토콜

(A) SSL (B) HTTPS

    Link State를 사용하며 라우터 자신을 네트워크 중심에 두고 최단 경로를 도출해 내는 라우팅 프로토콜

OSPF

    명령어를 실행하는 컴퓨터에서 목적지 서버로 가는 네트워크 경로를 확인해 주는 리눅스 명령어

traceroute

    C 클래스 사설 IP를 고르시오.

사설 A 클래스: 10.0.0.0 ~ 10.255.255.255
사설 B 클래스: 172.16.0.0 ~ 172.31.255.255
사설 C 클래스: 192.168.0.0 ~ 192.168.255.255

    RAID 구성에서 미러링 모드 구성이라고도 하며 디스크에 있는 모든 데이터는 동시에 다른 디스크에도 백업되어 하나의 디스크가 손상되어도 다른 디스크의 데이터를 사용할 수 있게 한 RAID 구성은?

RAID 1 (미러링)

### 2021년 2회

    네트워크 접속, 라우팅 테이블, 네트워크 인터페이스의 통계 정보를 보여주는 도구

netstat

    인터넷망과 같은 공중망을 사용하여 둘 이상의 네트워크를 안전하게 연결하기 위해서 가상의 터널을 만든 후 데이터를 전송할 수 있는 네트워크이다. 터널링 시 IP Sec, L2TP, PPTP 등의 프로토콜을 사용하기도 한다.

VPN

<details><summary>[해설]</summary>

- VPN에서 사용되는 2계층 프로토콜: L2F, L2TP, PPTP, SSL
- VPN에서 사용되는 3계층 프로토콜: IPSec
</details>

    IPv6에 관한 특징으로 알맞은 것을 모두 고르시오.

128비트, 헤더 이동성, 보안 서비스 향상, 유니캐스트, 멀티캐스트, 애니캐스트, 헤더 단순화

    전송계층에서 쓰는 프로토콜을 고르시오.

TCP, UDP

    다음 중 사설 B 클래스 주소로 유효한 것을 고르시오.
        (A) - 192.168.100.0
        (B) - 224.24.194,18
        (C) - 10.14.36.100
        (D) - 172.30.200.36

172.30.200.36

<details><summary>[해설]</summary>

사설 A 클래스 : 10.0.0.0 ~ 10. 255.255.255
사설 B 클래스 : 172.16.0.0 ~ 172.31.255.255
사설 C 클래스 : 192.168.0.0 ~ 192.168.255.255
</details>

### 2021년 1회

    Distance Vector를 사용하며 최대 지원 가능 홉 수가 15개이고 업데이트 주기는 30초인 라우팅 프로토콜

RIP

    웹 서비스와 주고받는 HTTP 트래픽을 필터링, 모니터링 및 차단하는 특정 형태의 애플리케이션 방화벽은 무엇인가?

웹 방화벽 (WAF)

    사설 IP를 공인 IP로 변경하는데 필요한 주소 변환 서비스는 무엇인가?

NAT

    현재 작업 중인 디렉터리의 이름을 출력하는 리눅스 명령어는 무엇인가?

pwd

    사설 A 클래스 주소인 것을 고르시오.
        (A) - 192.168.100.0
        (B) - 224.24.194,18
        (C) - 10.14.36.100
        (D) - 172.30.200.36

10.14.36.100

    드래그&드롭 문제(마우스로 드래그해서 그림에 맞게 넣으시오.)
        (A) - GNU 하에 개발된 리눅스 부트로더
        (B) - 리눅스 커널 부팅이 완료된 뒤 실행되는 첫 번째 프로세스다. 또한 커널이 직접 실행하는 유일한 프로세스
        (C) - 부팅 시 자동으로 마운트 되도록 설정해야 하는 파일

(A) grub (B) init (C) /etc/fstab

### 2020년 4회

    네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있도록 해주는 응용 프로그램 또는 프로토콜로 Telnet과 같은 프로토콜은 무엇인가?

SSH

    인터넷망과 같은 공중망을 사용하여 둘 이상의 네트워크를 안전하게 연결하기 위해 가상의 터널을 만든 후 데이터를 전송할 수 있는 네트워크이다.

VPN

    라우팅 테이블, 네트워크 인터페이스 또는 네트워크 연결을 보여주는 리눅스 명령어

netstat

    다음 중 ICMP Type 3번의 Message는 무엇인가?
        (A) - Redirect
        (B) - Destination Unreachable
        (C) - Echo Request
        (D) - Time Exeeded

(B) Destination Unreachable

<details><summary>[해설]</summary>

|Type|Message|
|:-:|:-|
|0|Echo Reply|
|3|Destination Unreachable|
|4|Source Quench|
|5|Redirect|
|8|Echo Request|
|11|Time exeeded|
</details>

    사설 C 클래스 주소인 것을 고르시오.
        (A) - 192.168.100.0
        (B) - 224.24.194,18
        (C) - 10.14.36.100
        (D) - 172.30.200.36

(A) 192.168.100.0

    RAID의 구성에서 미러링 모드 구성이라고도 하며 디스크에 있는 모든 데이터는 동시에 다른 디스크에도 백업되어 하나의 디스크가 손상되어도 다른 디스크의 데이터를 사용할 수 있게 한 RAID 구성은?

RAID 1

### 2020년 3회

    명령어를 실행하는 컴퓨터에서 목적지 서버로 가는 네트워크 경로를 확인해주는 리눅스 명령어

traceroute

    vtp는 연결된 스위치들끼리 정보를 주고 받아 자동으로 동기화하게 해주는 프로토콜인데, vtp에서 v의 의미는 무엇인가?

vlan

    대표적인 링크 상태 라우팅 프로토콜로 라우터 자신을 네트워크의 중심에 두고 최단 경로를 도출해 내는 프로토콜

OSPF

    외부 ping 차단 해제하기
        #echo (A) > icmp_echo_ignore_all

(A) 0

<details><summary>[해설]</summary>

0: 외부 ping 허용
1: 외부 ping 차단
</details>

    TCP/IP 4계층에서 인터넷 계층 프로토콜을 적으시오.

ARP, ICMP, IGMP

    IPv6 특징을 고르시오.

주소 길이 128bit, 8개의 비트로 필드 구성, 각 필드는 콜론으로 연결

### 2020년 2회

    A와 B 각각의 설명에 맞는 프로토콜을 고르시오.
        (A) - 브라우저 사이에 전송되는 데이터를 암호화하여 인터넷 연결을 보호하기 위한 기술
        (B) - 웹사이트가 SSL/TLS 인증서로 보호되는 HTTP 통신을 하는 프로토콜

(A) SSL (B) HTTPS

    리눅스에서 사용자는 파일에 모든 권한을 가지고 그룹 및 다른 사용자에게는 읽기, 실행 권한만을 부여하도록 접근 권한을 설정하는 법

chmod 755 [파일 이름]

    패킷이 라우팅 되는 경로의 추적에 사용되는 유틸리티로 목적지 경로까지 각 경유지의 응답 속도를 확인할 수 있는 것은?

tracert

<details><summary>[해설]</summary>

tracert: icmp 기반
traceroute: udp 기반
</details>

    192.xxx.xxx.xxx/26의 서브 네트워크 수 구하기

4개

<details><summary></summary>

/26 >
11111111.11111111.11111111.11000000 >
255.255.255.(128+64) > 255.255.255.192
</details>

    C 클래스에서 유효한 사설 IP 주소는 무엇인가?
        (A) - 33.114.17.24
        (B) - 128.46.83.25
        (C) - 192.168.13.87
        (D) - 222.248.255.34

(C) 192.168.13.87

    드래그&드롭 문제
        (A) - 사설 IP를 공인 IP로 변경에 필요한 주소 변환 서비스
        (B) - 네트워크에 연결된 장치에 IP 주소를 자동으로 할당하기 위해 사용되는 네트워크 관리 프로토콜
        (C) - 네트워크 트래픽을 모니터링하고 제어하는 네트워크 보안 시스템
        (D) - 인터넷망과 같은 공중망을 사용하여 둘 이상의 네트워크를 안전하게 연결하기 위해서 가상의 터널을 만든 후 데이터를 전송할 수 있는 네트워크이다.

(A) NAT (B) DHCP (C) 방화벽(Firewall) (D) VPN

### 2019년 4회

    Distance Vector를 사용하며 최대 지원 가능한 홉 수가 15개이고 업데이트 주기가 30초인 라우팅 프로토콜은?

RIP

    네트워크 상 다른 컴퓨터에 접속하거나 원격으로 네트워크에 접근할 때 시스템 간의 파일을 복사해서 동기화 시켜주는 응용 프로토콜은 무엇인가

SSH

    리눅스에서 주어진 명령어의 매뉴얼(도움말, 옵션 등)을 출력하기 위해 사용하는 명령어는?

man

    TCP의 특징으로 알맞은 것을 모두 고르시오.
        (A) - 연결성
        (B) - 비연결성 
        (C) - 신뢰성
        (D) - 송신과 수신이 다르다.
        (E) - 송신과 수신이 동일하다.

(A), (C), (E)

    192.168.100.150/25의 네트워크 ID를 구하시오.

192.168.100.128

    RAID의 구성에서 미러링 모드 구성이라고도 하며 디스크에 있는 모든 데이터는 동시에 다른 디스크에도 백업되어 하나의 디스크가 손상되어도 다른 디스크의 데이터를 사용할 수 있게 한 RAID 구성은?

RAID 1

### 2019년 3회

    172.168.200.100/18의 네트워크 ID를 구하시오.

172.168.192.0

    OSI 7계층에서 코드화, 암호화, 복호화, 압축, 인증을 수행하는 계층은 무엇인가?

표현계층

    리눅스에서 가장 우선순위가 높은 프로세스를 보여주는 명령어는 무엇인가?

top

    IP 주소의 낭비를 막기 위해 모든 호스트에 공인 IP 주소를 설정하고 내부적으로 사설 IP를 설정하며 

NAT

    인터넷 그룹 관리 프로토콜로 컴퓨터가 멀티캐스트 그룹을 인근의 라우터들에게 알리는 수단을 제공하는 인터넷 프로토콜은?

IGMP

    사설 A 클래스 주소로 유효한 것은?
        (A) - 192.100.2.134
        (B) - 10.172.192.24
        (D) - 172.18.244.100
        (E) - 224.45.67.129

(B) 10.172.192.24

<details><summary>[해설]</summary>

사설 A 클래스 : 10.0.0.0 ~ 10. 255.255.255
사설 B 클래스 : 172.16.0.0 ~ 172.31.255.255
사설 C 클래스 : 192.168.0.0 ~ 192.168.255.255
</details>

### 2019년 2회

    '스위칭'이라는 LAN 기술을 기반으로 물리적 시간만 고려되었던 LAN 분야에 가상이라는 개념을 도입한 것이다. 네트워크 구성에 대한 지리적 제한을 최소화하면서 사용자가 원하는 최대한의 논리적인 네트워크를 구성할 수 있도록 수단을 제공하는 기술은 무엇인가?

VLAN

    라우팅 테이블, 네트워크 인터페이스 또는 네트워크 연결을 보여주는 리눅스 명령어

netstat

    init은 리눅스 기본 프로세스이다. 시스템을 종료하기 위한 init 옵션은 무엇인가?

0

    172.50.48.6/255.255.224.0의 네트워크 ID를 구하시오.

172.50.32.0

    다음 중 IPv6에 대한 설명으로 옳은 것을 모두 고르시오.
        (A) - 주소 크기가 64비트이다.
        (B) - 헤더 확장으로 이동성이 좋아졌다.
        (C) - 유니캐스트, 애니캐스트, 멀티캐스트
        (D) - 유니캐스트, 브로드캐스트, 멀티캐스트
        (E) - 브로드캐스트
        (F) - 보안성이 좋아졌다.

(B) (C) (F)

    TCP/IP 4계층에서 인터넷 계층인 프로토콜을 모두 고르시오.
        (A) - ARP
        (B) - SMTP
        (C) - TCP
        (D) - IGMP
        (E) - ICMP
        (F) - HTTP

(A) ARP, (D) IGMP, (E) ICMP


### 출제 빈도순 정리 (기출 회차 랜덤)

1. TCP/IP 4계층의 각 계층에 속하는 프로토콜을 고르시오.

    네트워크 액세스 계층: 리피터, 허브, FDDI, HDLC, ISDN, ATM, WIFI  
    인터넷 계층: ARP, RARP, IGMP, ICMP  
    전송 계층: TCP, UDP  
    응용 계층: SMTP, HTTP, POP3, FTP, DHCP, DNS, Telnet  

2. 주어진 IP, Subnet mask에 대해 네트워크 ID를 구하시오.
- [유형1] 172.168.102.5/19
```
/19: 11111111.11111111.11100000.00000000
    = 255.255.224.0
호스트 개수: (256 - 224) = 32 개
주소 범위:
    172.168.0.0 ~ 172.168.31.255
    172.168.32.0 ~ 172.168.63.255
    172.168.64.0 ~ 172.168.95.255
    172.168.96.0 ~ 172.168.127.255 <- "172.168.102.5/19" 포함 범위!
따라서, 네트워크ID는 172.168.96.0
```
- [유형2] B 클래스이며 IP는 172.168.102.5이고 Subnet mask는 255.255.224.0일 때,
```
호스트 개수: 256 - 224 = 32 개
주소 범위:
    172.168.0.0 ~ 172.168.31.255
    172.168.32.0 ~ 172.168.63.255
    172.168.64.0 ~ 172.168.95.255
    172.168.96.0 ~ 172.168.127.255 <- "172.168.102.5/19" 포함 범위!
따라서, 네트워크ID는 172.168.96.0
```
- [유형3] 162.128.0.0 ~ 162.128.255.255의 네트워크 ID 값을 나타내는 것을 적어라.

|클래스<br>구분|주소 영역 구분<br>(네트워크+호스트)|서브넷 마스크|식별용 상위 비트|주소 범위|
|:-:|:-:|:-:|:-:|:-:|
|A|[네트워크][호스트][호스트][호스트]|255.0.0.0(/8)|0XXXXXXX|0.0.0.0 ~<br>127.255.255.255|
|B|[네트워크][네트워크][호스트][호스트]|255.0.0.0(/16)|10XXXXXX|128.0.0.0 ~<br>191.255.255.255|
|C|[네트워크][네트워크][네트워크][호스트]|255.0.0.0(/24)|110XXXXX|192.0.0.0 ~<br>223.255.255.255|
|D| 멀티캐스트용 |-|1110XXXX|224.0.0.0 ~<br>239.255.255.255|
|E| 실험용 |-|1111XXXX|240.0.0.0 ~<br>255.255.255.255|

3. 인터넷망과 같은 공중망을 사설망처럼 이용해 회선 비용을 크게 절감할 수 있는 기업통신 서비스는?
```
    VPN(가상사설망)
```
4. IPv4와 IPv6을 비교하는 문제
```
[IPv4]
    8비트 * 4개 = 32비트 주소 체계
    10진수로 이루어짐
    유니캐스트, 멀티캐스트, 브로드캐스트
[IPv6]
    16비트 * 8개 = 128비트 주소 체계
    16진수로 이루어짐
    유니캐스트, 멀티캐스트, 애니캐스트
```
5. IDS와 IPS 개념 문제
```
IDS (Intrusion Detection System): 침입 탐지 시스템  
    - 오용 탐지 기법
        비정상 행위를 정의해서 대항 행동을 찾음
        새로운 침입 유형의 탐지가 불가능함
    - 비정상 탐지 기법
        정상 행위를 정의해서 이를 벗어나는 행동을 찾음
        새로운 침입 유형의 탐지가 가능함
IPS (Intusion Prevention System): 침입 방지 시스템
    - 탐지 포함 탐지된 공격에 대한 새로운 솔루션을 제공하는 시스템
```
6. 네트워크 스위치의 어떤 포트에서 보이는 모든 네트워크 패킷이나 전체 VLAN의 패킷들을 다른 모니터링 포트로 복제하는 데 사용하는 것은?
```
포트 미러링
```
7. 각 계층별로 장치를 연결하시오.
```
물리계층     - 허브, 리피터
데이터링크   - 스위치, 브리지
인터넷 계층  - 라우터
그 외 계층   - 게이트 웨이
```
8. IP 주소를 물리적 네트워크 주소인 MAC 주소로 대응시키기 위해 사용되는 프로토콜은?
```
ARP
```
9. IP 주소를 사용하는 것의 낭비를 막기 위해 모든 호스트에 공인 IP 주소를 설정하는 대신, 내부적으로 사설 IP를 설정하여 사용하고, 인터넷에 접속할 때만 공인 IP로 변환하는 기술은?
```
NAT
```
10. 2개 이상의 다른 혹은 같은 종류의 통신망을 상호 접속하여 통신망 간 정보를 주고 받을 수 있게 하는 기능의 단위나 장치는?
```
게이트웨이
```
11. 사설망 IP에 사용되는 주소
```
Class A: 10.0.0.0 - 10.255.255.255
Class B: 172.16.0.0 - 172.31.255.255
Class C: 192.168.0.0 - 192.168.255.255
```
12. 데이터링크 계층 중 MAC 계층에서 일하며 두 세그먼트 사이에서 데이터링크 계층 간의 패킷 전송을 담당하는 장치는?
```
브리지
```
13. RFID와 관계가 없는 것은?
```
보기 - RFID 리더기, 태그, 안테나, Access point
답 - Access point
```
14. 매니지먼트와 에이전트 사이에 관리 정보를 주고받기 위한 프로토콜이며 정보 교환 단위는 메시지이다.
```
SNMP
```
15. TCP/IP 기반 네트워크 상에서 서버나 라우터가 에러나 예상치 못한 사건들을 보고할 목적으로 만들어진 프로토콜?
```
ICMP
```
16. 동적 디스크 볼륨 중 볼륨 자체에서 하나의 디스크가 손상되었을 경우, 다른 하드디스크의 데이터로 손상된 부분을 복구할 수 있는 내 결함성을 지원하는 볼륨은?
```
미러링
```
17. (A)와 (B)는 무엇인가?
```
스푸핑: (A)는 임의로 구성된 웹사이트에서 눈속임을 통해 이용자의 정보를 빼가는 해킹 수법의 하나이다.
SSH: (B)는 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 다른 시스템으로 파일을 복사할 수 있게 해주는 응용 프로그램이다.
```
18. 무슨 클래스에 대한 설명인가?
```
class IP Address는 처음 1개의 bit가 항상 0이다.
할당 가능한 네트워크 수는 2^7, 127 개이다.
할당 가능한 호스트의 수는 2^24 개이다.
→ A class
```
19. 네트워크에서 공격 서명을 찾아내서 자동으로 조치를 취함으로써 비정상적인 트래픽을 중단시키는 보안 솔루션은?
```
IPS
```
20. 이미 발견되어 있는 공격 패턴을 미리 입력하고, 해당하는 패턴을 탐지하는 기법은?
```
오용탐지기법
```

## 라우터 유형 기출

### 2022년 3회차

1. Router1의 FastEthernet 0/0을 사용 가능하게 IP 주소를 192.168.0.101/24와 두번째 IP를 192.168.102/24로 설정하고 활성화하시오.
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# ip add 192.168.0.101 255.255.255.0
Router(config-if)# ip add 192.168.102.0 255.255.255.0 secondary
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```
2. Router2의 DHCP pool을 icqa로 설정하고 네트워크는 192.168.100.0/24로 설정하시오.
```
Router> en
Router# conf t
Router(config)# ip dhcp pool icqa
Router(dhcp-config)# network 192.168.100.0 255.255.255.0
Router(dhcp-config)# exit
Router(config)# exit
Router# copy r s
```
3. 라우팅 테이블 정보를 확인하고 저장하시오.
```
Router> en
Router# show ip route
Router# copy r s
```

### 2022년 2회차

1. host의 정보를 확인하고 저장하시오
```
Router> en
Router# show host
Router# copy r s
```
2. SNMP 통신 시 ICQA라는 Community를 통해 모니터링 할 수 있도록 Router1에 설정하시오. (단, 'ICQA'는 대소문자를 구분함. 완료된 설정은 'Router# copy running-config  startup-config'를 사용하여 startup-config에 저장하고 이외 저장 명령어는 사용 금지)
```
Router> en
Router# conf t
Router(config)# snmp community ICQA
Router(config)# exit
Router# copy r s
```
3. 정적 라우팅을 설정하시오.
> 목적지 네트워크 IP: 24.48.200.0/24  
게이트웨이 IP: 100.150.100.2
```
Router> en
Router# conf t
Router(config)# ip route 24.48.200.0 255.255.255.0 100.150.100.2
Router(config)# exit
Router#copy r s
```

### 2022년 1회차

1. 라우터의 소프트웨어 버전과 IOS 버전 등을 확인하고 저장하시오.
```
Router> en
Router# show version
Router# copy r s
```
2. Default-Gateway를 설정하고, 저장하시오.
> IP 192.168.100.0
```
Router> en
Router# conf t
Router(config)# ip default-gateway 192.168.100.0
Router(config)# exit
Router# copy r s
```
3. FastEthernet 0/0의 Description을 설정하시오.
> Description: ICQA
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# description ICQA
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```

### 2021년 4회차

1. 인터페이스를 확인하고 저장하시오.
```
Router> en
Router# show interface
Router# copy r s
```
2. 라우터에서 telnet에 접속할 때 터미널 0부터 4까지 ssh로 접속할 수 있도록 설정하시오.
```
Router> en
Router# conf t
Router(config)# li vty 0 4
Router(config-if)# transport input ssh
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```
3. Router1의 FastEthernet 0/0을 사용 가능하게 IP 주소를 192.168.0.101/24와 두번째 IP를 192.168.102/24로 설정하고 활성화 하시오.
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# ip add 192.168.0.101 255.255.255.0
Router(config-if)# ip add 192.168.102.0 255.255.255.0 secondary
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```

### 2021년 3회차

1. 라우터의 소프트웨어 버전과 IOS 버전 등을 확인하고 저장하시오.
```
Router> en
Router# show version
Router# copy r s
```
2. ROUTER1의 Serial 2/0을 사용 가능하도록 IP주소를 192.168.0.101/24와 두번째 IP주소를 192.168.0.102/24로 설정하고 활성화하시오.
```
Router> en
Router# conf t
Router(config)# interface serial 2/0
Router(config-if)# ip add 192.168.0.101 255.255.255.0
Router(config-if)# ip add 192.168.102.0 255.255.255.0 secondary
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```
3. ROUTER 2의 DHCP 네트워크를 192.18.100.0/24 서버 이름은 'icqa'로 설정하시오.
```
Router> en
Router# conf t
Router(config)# ip dhcp pool icqa
Router(dhcp-config)# network 192.168.100.0 255.255.255.0
Router(dhcp-config)# exit
Router(config)# exit
Router# copy r s
```

### 2021년 2회차

1. 라우팅 테이블 정보를 확인하고 저장하시오.
```
Router> en
Router# show ip route
Router# copy r s
```
2. 기본 게이트웨이를 설정하시오.
> IP 192.168.0.10
```
Router> en
Router# conf t
Router(config)# ip default-gateway 192.168.0.10
Router(config)# exit
Router# copy r s
```
3. ROUTER2의 serial 2/0에 frame relay 방식으로 캡슐화하시오.
```
Router> en
Router# conf t
Router(config)# interface serial 2/0
Router(config-if)# encapsulation frame-relay
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```

### 2021년 1회차

1. 접속한 사용자 정보를 확인하고 저장하시오.
```
Router> en
Router# show user
Router# copy r s
```
2. FastEthernet 0/0의 정적 라우팅 테이블 경로를 지정하시오.
> 목적지 IP: 24.48.200.0/24  
게이트웨이 IP: 100.150.100.2
```
Router> en
Router# conf t
Router(config)# ip route 24.48.200.0 255.255.255.0 100.150.100.2
Router(config)# exit
Router# copy r s
```
3. FastEthernet 0/0의 Description을 설정하시오.
> Description: ICQA
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# description ICQA
Router(config-if)# exit
Router(config)# exit
Router# copy r s
```

### 2020년 4회차

1. 모든 인터페이스 확인
```
Router> en
Router# show interface
Router# copy r s
```
2. 기본 네트워크를 설정하시오.
> IP 192.168.0.10
```
Router> en
Router# conf t
Router(config)# ip default-network 192.168.10.0 
Router(config)# exit
Router# copy r s
```
3. FastEthernet 0/0에 IP를 추가한 후 활성화하시오.
> IP 192.168.100.1/24
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# ip add 192.168.100.0 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# end
Router# copy r s
```

### 2020년 3회차

1. 라우터 버전 확인
```
Router> en
Router# show version
Router# copy r s
```
2. serial 2/0에 encapsulation frame-relay 설정
```
Router> en
Router# conf t
Router(config)# interface serial 2/0
Router(config-if)# encapsulation frame-relay
Router(config-if)# end
Router# copy r s
```
3. ROUTER2의 Serial 0/0의 IP 및 서브넷을 설정하고 저장하시오.
> IP 192.168.100.1/24
```
Router> en
Router# conf t
Router(config)# int s 2/0
Router(config-if)# ip add 192.168.100.1 255.255.255.0
Router(config-if)# end
Router# copy r s
```

### 2020년 2회차

1. 라우팅 테이블 확인
```
Router> en
Router# show ip route
Router# copy r s
```
2. ROUTER2의 Serial 2/0의 IP 및 서브넷을 설정하고 저장하시오.
> IP 192.168.100.1/24
```
Router> en
Router# conf t
Router(config)# int s 2/0
Router(config-if)# ip add 192.168.100.1 255.255.255.0
Router(config-if)# no sh
Router(config-if)# end
Router# copy r s
```
3. Default-Network를 설정하고 저장하시오.
> IP 192.168.100.0
```
Router> en
Router# conf t
Router(config)# ip default-network 192.168.10.0
Router(config)# end
Router# copy r s
```

### 2019년 4회차

1. 라우터의 소프트웨어 버전과 IOS 버전 등을 확인하고 저장하시오.
```
Router> en
Router# show version
Router# copy r s
```
2. Default-Gateway를 설정하고 저장하시오.
> IP 192.168.100.0
```
Router> en
Router# conf t
Router(config)# ip defalt-gateway 192.168.100.0
Router(config)# exit
Router# copy r s
```
3. ROUTER2의 Serial 2/0에 frame relay 방식으로 캡슐화하시오.
```
Router> en
Router# conf t
Router(config)# int s 2/0
Router(config-if)# encapsulation frame-relay
Router(config-if)# end
Router# copy r s
```

### 2019년 3회차

1. 접속한 사용자 정보를 확인하고 저장하시오.
```
Router> en
Router# show user
Router# copy r s
```
2. ROUTER1의 FastEthernet 0/0을 사용 가능하게 IP 주소를 192.168.0.101/24와 두번째 IP 주소를 192.168.102/24로 설정하고 활성화하시오.
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# ip add 192.168.0.101 255.255.255.0
Router(config-if)# ip add 192.168.102.0 255.255.255.0 secondary
Router(config-if)# no sh 
Router(config-if)# end
Router# copy r s
```
3. ROUTER2의 호스트 이름을 'ICQA'로 설정하시오.
```
Router> en
Router# conf t
Router(config)# hostname ICQA
Router(config)# exit
Router# copy r s
```

### 2019년 2회차

1. 라우팅 테이블을 확인하고 저장하시오.
```
Router> en
Router# show ip route
Router# copy r s
```
2. ROUTER2의 DHCP 네트워크를 192.168.100.0/24로 서버 이름은 'icqa'로 설정하시오.
```
Router> en
Router# conf t
Router(config)# ip dhcp pool icqa
Router(dhcp-config)# network 192.168.100.0 255.255.255.0
Router(dhcp-config)# end
Router# copy r s
```
3. FastEthernet 0/0의 Description을 설정하시오.
> Description: ICQA
```
Router> en
Router# conf t
Router(config)# interface fastethernet 0/0
Router(config-if)# description ICQA
Router(config-if)# end
Router# copy r s
```

-------------------------------------------------
```
Router> en
Router# conf t
Router(config)# 
Router(config)# exit
Router# copy r s
```
-------------------------------------------------

### 기능별 기출 1: 인터페이스(fastethernet/serial)

1. serial 2/0의 대역폭을 2048로 설정하시오.

2. serial 2/0의 clock rate를 72k로 설정하시오.

3. fastethernet 0/0의 description을 ICQA로 설정.

4. router2의 interface serial 0/0를 활성화시키고 저장하시오.

5. serial 2/0에 frame relay 방식으로 캡슐화 하시오.

6. 아래와 같이 인터페이스를 설정하시오.

7. fastethernet 0/0 의 ip address를 192.168.2.1/30와 192.168.3.1/30 secondary로 설정하고 저장하시오.

8. router1에 access-list 1이 설정되어 있을 때, fastethernet 0/0에 적용하시오.

### 기능별 기출 2: 텔넷/콘솔(telnet/console)

1. router1 telnet에 접근하는 password를 "telpass"로 설정하고 상태를 저장하시오.

2. telnet에 5분 50초 동안 신호가 없을 시 해당 세션을 자동으로 종료하도록 라우터를 설정하시오.

3. router1 console의 패스워드를 ICQACon으로 설정하고 저장하시오.

4. hostname을 network2로 변경하고, console 0의 password를 router5로 변경하고 login하라.

### 기능별 기출 3: 확인(show)

1. 인터페이스 정보를 확인하라

2. 접속한 사용자를 확인하라

3. 라우팅 테이블을 확인하라

4. 플래쉬를 확인하라

5. 소프트웨어 버전과 IOS 버전등을 확인하라

6. CPU process list를 확인하라

### 기능별 기출 4: 전역 설정

1. default-gateway를 설정하고 저장하시오. (ip: 192.168.0.10)

2. 192.168.1.1 네트워크로 default network를 설정하시오.

3. 호스트 네임을 ICQA로 변경하라
