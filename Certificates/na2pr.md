---
updated:
    2024-09-05
tags:
    - CERTIFICATE
---

<link rel="stylesheet" href="../Assets/style.css" />

# Network Administrator

- National Accreditation Certificate Level 2

## 입문

- 우선순위?
    + 에뮬레이터★★★ > 라우터★★ > 케이블★
- 일반적으로 크로스 케이블보다 다이렉트 케이블 출제 빈도가 더 높음
- 윈도우 에뮬레이터 서비스 문제는 항목이 많아서 틀리는 경우가 많음

### 라우터 에뮬레이터

#### 라우터 IP/서브넷 마스크 설정

```
문제) ROUTER 1의 FastEthernet 0/0의 IP를 192.168.0.100/24로 설정하시오.
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

#### 라우터 대역폭 설정

> 저장 명령어는 모든 문제가 동일함 → 설정 명령어 부분 집중 숙지

```
문제1) ROUTER2의 Serial 2/0의 대역폭을 2048로 설정하시오.
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
문제2) ROUTER1의 Serial 2/0의 클럭 속도를 72K로 설정하시오.
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

#### 라우터 주석 설정

```
문제) FastEthernet 0/0의 Description을 설정하시오.
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

#### 라우터 Secondary IP Address 설정

```
문제) ROUTER1의 Serial 2/0을 사용가능하게 IP 주소를 192.168.0.101/24와 두번째 IP 192.168.0.102/24로 설정하고 활성화 하시오.
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

#### 라우터 기본 게이트웨이

```
문제1) 기본 게이트웨이를 설정하시오. IP: 192.168.0.10
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
문제2) ROUTER1의 DHCP 네트워크를 192.168.100.0/24 서버 이름은 'icqa'로 설정하시오. 
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

#### 라우터 텔넷 패스워크 설정

```
문제) ROUTER1 Telnet에 접근하는 Password를 icqa로 설정하고 로그인 하시오.
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

#### 라우터 텔넷 세션 자동종료 설정

```
문제) 텔넷 연결 후 3분 50초 동안 입력이 없으면 세션이 자동 종료 되도록 설정하시오.
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

#### 라우터 콘솔 패스워드 설정

```
문제) ROUTER1 console 0의 패스워드를 ICQA로 설정하고 로그인하시오.
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

#### 라우터 활성화 설정

```
문제) ROUTER 1 Serial 2/0을 활성화 시키시오.
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

#### 라우터 호스트 네임 설정

```
문제) Hostname을 network2로 변경하고 console 0의 password를 route5로 변경한 후 로그인 하시오.
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

#### 라우터 확인문제 유형 분석

```
문제) 인터페이스 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en  # 설정을 변경해야 하는 문제가 아니면 전역모드로 들어갈 필요 없음!
show interface  # show ~ : 정보 확인
copy r s
```
```
문제) 접속한 사용자 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show user
copy r s
```
```
문제) 라우팅 테이블 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en conf t
show ip route
copy r s
```
```
문제) 플래쉬 내용을 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show flash
copy r s
```
```
문제) 프로세스 정보를 확인하고 저장하시오.
```
```sh
# ROUTER 선택
en
show process
copy r s
```

### UTP 케이블 제작

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

### 윈도우 에뮬레이터

#### 윈도우 서버 TCP/IP 설정

```
문제) 네트워크 환경에 <제시 문제>와 같이 IP 주소와 서브넷 마스크를 추가하시오.
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

#### 윈도우 서버 DNS 설정

```
문제) 관리도구에 <제시 문제>와 같이 DNS 서버를 설정하시오.
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

#### 윈도우 서버 DHCP 설정

```
문제) 아래의 <보기>와 같이 IP를 할당할 수 있는 DHCP를 설정하시오.
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

#### 윈도우 서버 IIS FTP 서버 설정

```
문제) 아래의 <보기>와 같이 FTP 사이트를 추가 및 설정하시오.
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

#### 윈도우 서버 IIS 웹사이트 추가 및 설정

```
문제) 아래의 [보기]와 같이 Web 사이트를 추가 설정하시오.
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

#### 윈도우 서버 계정 추가

```
문제) 아래의 <보기>와 같이 계정을 추가 설정하시오.
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

#### 윈도우 서버 로컬보안정책 설정

```
문제) 아래의 <보기>와 같이 로컬보안정책을 설정하시오.
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

#### 윈도우 서버 서비스 설정

```
문제) 아래의 <보기>와 같이 서비스를 설정하시오.
원격 사용자가 Telnet을 이용하여 파일을 삭제하여 왔으나 정책이 변경되어 원격 사용자가 더 이상 로그온 할 필요가 없어졌다. 해당 기능을 중지시키고, 다시 시작할 수 없게 설정하시오.
```
```
0. 윈도우 에뮬레이터 실행
1. [관리도구] - [서비스 관리자]
2. [Telnet]
    + 사용안함
    + 중지
```