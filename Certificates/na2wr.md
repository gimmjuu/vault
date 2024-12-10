---
created:
    2024-08-06
tags:
    - CERTIFICATE
---

<link rel="stylesheet" href="../Assets/style.css" />
<style>
    h3 { padding: 7px 10px 8px; background-color: rgba(175, 239, 255, 0.05); border-radius: 3px; border: none; }
</style>

# Network Administrator

## CBT

### 2024.05.19

- DF Flag: 네트워크 계층 핵심 프로토콜 IP 단편화 작업 중 분할 데이터 구분자
- HTTPS : TLS를 통해 Application 계층 데이터를 암호화, 기본 포트 443
    - SSH : 기본포트 22
    - HTTP : 기본포트 80
- IGMP(Internet Group Management Protocol): 로컬 네트워크 상의 멀티캐스팅 그룹제어를 위한 프로토콜
- TCP 헤더 플래그 비트: URG, ACK, RST
    - UDP 헤더 플래그 비트: UTC
- 서브넷 마스트 255.255.255.192 => 64개 -1(마지막 한개는 브로드캐스팅용)
- UDP: 낮은 신뢰도, 비연결형 서비스
- SNMP
    - UDP 세션 사용
    - 네트워크 확장 용이
    - 주기적으로 풀링 -> 트래픽 주의
- ICMP 메시지 타입
    - 0: ECHO REPLY
    - 8: ECHO REQUEST
- TCP 사용 프로토콜
    - FTP, Telnet, SMTP
    - TFTP => UDP
- OSFP 프로토콜 최단경로 검색 알고리즘
    - Dijfkstra 알고리즘
- IP 체크섬(Checksum)
    - ip 헤더의 완전성 검사
    - 데이터 검사 XXX
- DHCP 적용 장비 => 교육장용 pc (가변ip)
    - 고정 ip -> 웹서버, access point, 네트워크 프린터
- IPv16
    - 128, 유니|애니|멀티캐스트
- tcp/ip 프로토콜 응용 계층, 원격장치 설정, 네트워크 사용 감시 관리
    - SNMP
- 라우터 및 스위치 장비 시간 동기화, 123번 포트
    - SNTP
- 인터넷 전자우편 표준 프로토콜
    - SMTP = 25번
    - telnet = 23번
- DNS 서버가 없는 그룹웨어, 로컬 DNS 설정 파일
    - /etc/hosts
- 데이터 흐름 제어
    - Sliding Window, XON/XOFF, Stop and Wait
    - Loop/Echo -> 네트워크 테스트 및 디버깅
- 엣지 컴퓨팅 -> 포그 컴퓨팅
- 홉 사이의 세부 시간 정보를 저장하고 있음 -> pathping
- linux에서 daemon이 살아있는지 확인 -> px
- linux runlevel
    - 0: 시스템 종료
    - 3: 다중 사용자 모드, 텍스트 모드
    - 5: 다중 사용자 모드, GUI 모드
    - 6: 시스템 재부팅
- MAC 주소: L2 LAN 스위치가 이더넷 프레임을 중계 처리할 때 사용
- 내부 통신에는 사설 ip 사용, 외부 통신에는 공인 ip 사용 -> NAT
- 광 케이블 - 내부 코어와 외부 클래딩(굴절률이 다른 유리나 플라스틱)으로 구성된 전송 매체
- RAID 0 : 2개 이상의 하드 디스크, 하드디스크 동시 저장, 속도 가장 빠름, Stripping
- utp 케이블로 전원 연결 및 데이터 전송이 동시에 가능, cctv 등에 사용 -> POE Switch

### 2024.02.25

- ip 헤더 필드: version, header checksum, header length
- tcp 헤더 필드: ACK
- tcp 프로토콜 흐름제어 방식: sliding window
- 서브넷 마스크(네트워크 영역을 분리 또는 합체)
    - 기본적으로 255와 0으로 구성
    - 255는 네트워크 부분, 0은 호스트 부분
    - 255 부분은 무시하고, 0 부분에서 IP를 쪼개는 개념
    - 네트워크수는 2의 제곱으로 계산
    - 호스트 수 = 256 / 네트워크수 -2(..., 브로드캐스트용)
    - [128 64 32 16 8 4 2 1]
- Link State 알고리즘 네트워크 내 통신 프로토콜 -> OSPF
- DNS에서 TTL(Time To Live)? 데이터가 DNS 서버 캐시로부터 나오기 전에 현재 남은 시간
- RIP(Routing Information Protocol): Hop Count 만 고려함
- ARP: IP Address를 이용해 물리적인 네트워크 주소를 얻음
- OSI 계층별 PDU(Protocol Data Unit)
    - 1 계층-물: 비트
    - 2 계층-데: 프레임
    - 3 계층-네: 패킷
    - 4 계층-전: [tcp]세그먼트 [udp]데이터그램
    - 5 계층-세: 메시지 
    - 6 계층-표: 메시지
    - 7 계층-응: 메시지
- CSMA/CD
    - 케이블의 데이터 흐름 유무를 감시하기 위해 특정 신호를 주기적으로 보냄
    - 충돌 시 일정 시간 대기 후 재전송
- tocken-ring: 반드시 'token'이라는 권한을 가져야 함
- ARP(Address Resolution Protocol): IP 주소 -> MAC 주소(하드웨어 주소)
- RARP(Reverse Address Resolution Protocol): MAC 주소 -> IP 주소
- IPv6 주소(8옥탯)에서 ':0000:'는 '::'으로 생략 가능, "::"은 주소에서 한번만 작성
    - 상수/문자 앞 '0' 생략 가능, 뒤 '0' 생략 불가
- 사설망 : 172.xxx.xxx.xxx

<details>
  <summary>사설 IP/공인 IP/NAT 기본 개념</summary>

* [참고](https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-IP-%EA%B8%B0%EC%B4%88-%EC%82%AC%EC%84%A4IP-%EA%B3%B5%EC%9D%B8IP-NAT-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A7%90-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC)

- 고정 IP: 컴퓨터에 고정적으로 부여된 IP
- 유동 IP: 임시 발급이 가능한 변하는 IP
- 공인 IP: ICANN에서 KISA에, KISA에서 ISP에, ISP에서 개인 사용자에게 부여한 IP
- 사설 IP: 어떤 네트워크 안에서 내부적으로 사용되는 고유 주소, 로컬 IP
    - 10.x.x.x
    - 192.168.0.x
</details>

- Proxy: 서버와 클라이언트 사이 중계기, 캐시기능
- ICMP(Internet Control Message Protocol): IP 데이터그램 프로세싱 동안 오류 보고
- IGMP: 비대칭 프로토콜, TimeToLive 제공
- Internet Data Center: 전용 회선 및 전력 공급 안정화
- NAC: 단말이 네트워크에 접근하기 전 보안 검사, 보안 위협 정도에 따라 제어
- Go-Back-N ARQ : 에러 발생 블럭으로 돌아가 모두 재전송
- 표현계층 -> text 압축, 암호화
- FTTP 망구조
    - PON: 가입자들이 동일한 광신호 수신
    - AON, PTP, Homerun : 각 가입자가 독립적인 광섬유 연결을 가짐
- 채널 부호화: Convolutional Code
- 블록 부호화: ADPCM, ADM, PCM
- CR(Cognitive Radio): 비어있는 주파수를 검색하여 사용하는 기술
- Linux 시스템 디렉터리
    - /etc: 시스템 설정 정보, 사용자 암호 정보 파일(/etc/shadow)
- TCP 3way-handshaking: SYN(LISTEN) -> SYN-RECEIVED -> SYN
- shell: linux 명령어 해석기
- GRUB: 멀티부팅 지원 linux 로더
- EFS(암호화 파일 시스템)
- DNS 질의 -> 재귀적 질의
- windows container < Hyper-V virtual machine
- RAID 1: 미러링, 최고 성능, 고장대비고효율
- 로드밸런싱 = 최적화
- SDN = 가상 구성 근거리 통신망
- VM = 가상머신 컴퓨터
- HTTPS = 통신 정보 암호화
- 게이트웨이(Gateway): 전송계층 장비이지만 모든 계층에서 작동 가능
- IEEE 802.11 무선랜 전송
    - 주파수 방식 = 전파 이용 방식(스프레드 스펙트럼), 일반적인 무선랜 방식

### 2023.11.05

- Hop Limit: IPv6 헤더 데이터그램 생존 기간
- DHCP(Dynamic Host Configuration Protocol): 동적 호스트 설정 프로토콜
- ARP: 주소 결정 프로토콜
- NAT: 사설 ip 주소를 공인 ip 주소로 바꿔줌
- 네트워크 점검 명령어
    - tracert: 네트워크 상세 확인 windows
    - traceroute: 네트워크 상세 확인 linux
    - ping: 네트워크 상태 점검
    - nslookup: 도메인 정보 조회
- IPv4 127.~ : 루프백 주소
- L4 switch -> 로드밸런싱(Load Balancing)
- MTU(Maximum Transmission Unit)
    - IPv4 기준 최소 63 바이트
    - IPv6 기준 최소 1280 바이트
    - Ethernet 구현 시 대부분 Ethernet V2 프레임 형식, 1500 바이트 고정
- 데이터링크계층: 에러 제어
- 세션계층: 대화제어, 연결 설정 종료, 동기화
- WPA2(IEEE 802.11i): 가장 강력한 무선 LAN 보안
- IEEE 802.15.1: 블루투스 관련
- IEEE 802.15.3: 고속 WPAN
- IEEE 802.15.4: 저속 WPAN
- Bit Locker: 도난 발생 시 데이터 암호화
- httpd.conf: Apache 서버 설정파일
- Windows 이벤트 뷰어 표준 이벤트 수준
    - 경고
    - 오류
    - 정보
    - 위험
    - 자세한 정보 표시
- 분 시 일 월 요일 쉘스크립트파일
- MAC 주소: (8*8) -> 48 비트
- MPLS -> L2 switching
- sconfig: windows 서버 코어에서 숫자로 기본서버 설정

### 2023.08.20

- DNS: TCP와 UDP 포트를 함께 사용하는 프로토콜
- SNMP: 네트워크 장비의 관리 및 감시
- HTTP 상태 코드 그룹
    - 100번대: 정보 제공
    - 200번대: 성공
    - 300번대: 리다이렉션
    - 400번대: 클라이언트 에러
    - 500번대: 서버 에러
- 멀티캐스트 -> D Class
- 캡슐화 : 헤더와 꼬리를 붙이는 것
- TCP 3Way Handshaking
    1. SYN
    2. ACK+SYN
    3. ACK
- Quality of Service: IP 및 프로토콜이 장비를 반드시 통과하여 인터넷 전용회선의 대역폭을 이벤트에 알맞도록 조정할 수 있음
- 표현계층: 암호, 복호, 인증
- WDM(파장분할다중화방식): 광증폭기를 사용해 무중계 장거리 전송이 가능
- FTP
    - passive mode: 클라이언트가 서버로 접속
    - active mode: 서버가 클라이언트로 접속
- Round Robin: 여러개의웹서버를 운영하여 dns 서버에서 ip 주소를 돌아가며 알려줌
- crontab
    - u: user
    - e: edit
    - l: list
    - r: remove
- SWAP: 논리적인 메모리 저장공간
- certmgr.msc: WINDOWS SERVER 인증서 관리자 호출
- Multihoming: 인터넷 다중 접속 기술
- MAC 주소: L2 LAN 스위치가 이더넷 프레임을 중계 처리할 때 사용하는 주소
- 게이트웨이: 서로다른 인터페이스를 가진 네트워크들의 중간 지점
- RAID 0: 하나의 디스크만 손상되어도 전체 복구가 어려움
- VLAN: 하나의 물리적인 네트워크 스위치를 여러개의 네트워크 스위치처럼 나눠쓰는 것

### 2023.05.21

- D CLASS: 멀티캐스트 용도
- DNS TTL: 데이터가 DNS 서버 캐시로부터 나오기 전 현재 남은 시간
- RARP: Reversed ARP
- Echo Request: 8
- TCP/IP 4계층
    - APPLICATION
    - TRANSPORT
    - INTERNET
    - NETWORK INTERFACE
- ZigBee: 저전력, 저가격, 근거리, IEEE 802.15.4
- Adaptive ARP: 적응형 ARP, 프레임 길이를 동적으로 변경
- 표현계층: TEXT 압축, 암호화
- IPSec: VPN 터널링 프로토콜
- SIP(Session Initation Protocol): 시그널링 프로토콜, IP 주소에 종속되지 않고 서비스를 받을 수 있음
- 기가비트 이더넷 규격: 1000BASE-T, 1000BASE-SX, 1000BASE-LX, 1000BASE-CX, 1000BASE-TX
- 핸드오프(핸드오버): 현재 사용 중인 셀에서 다른 셀로 이동 시, 끊김 없도록 통화 채널을 바꿈
- DNS CNAME: 별칭, 서브도메인 아님 주의
- Bit Locker: 메인보드와 BIOS에서 TPM 보안칩과 연동해야 함
- perfmon: Windows Server 성능 모니터 도구 시작 명령어
- lsattr: 해당 디렉터리 내 파일의 사용자 권한 확인 명령어
- nohup(no hang up): 터미널을 닫아도 실행 중인 프로세스를 백그라운드에서 작업
- ipconfig/flushdns : DNS cache 초기화 명령어
- Windows Server 도메인 사용자 계정 관리 명령어
    - dsadd
    - dsrm
- lastb: 비인가자 로그인 실패 시도 이력 확인 명령어
- xferlog: FTP 서버 전송 로그 확인 명령어
- history: 명령어 실행 이력 확인 명령어
- pkill: 프로세스를 신호로 종료시키는 명령어
- TCP 포드: 논리적인 포트
- IEEE 802.11
    - Wifi4 = 11n
    - Wifi5 = 11ac
    - Wifi6 = 11ax
- netstat: Linux 상 열려있는 port 정보를 확인하는 명령어
- getenforce: 현재 SELinux 모드 출력 명령어
- ps: 현재 실행 중인 프로세스를 확인하는 명령어
- pstree: 프로세스를 트리구조로 출력하는 명령어

### 2023.02.26

- 이웃 요청과 광고 메시지: ICMPv6에서 특정 호스트로의 전달 가능 여부 검사용 메시지
- Sliding Window: TCP 수신측 버퍼 크기에 맞춰 데이터 크기를 송신측에서 적절하게 조절할 수 있는 필드
- IGMP: 인터넷 멀티캐스트용 프로토콜
- TTL: 살아서 거친 라우터 개수! (초 단위 시간 값 X)
- HTTPS: 기본포트 443
- Wireless Mesh Network: 애드 혹 네트워크, 무선 분산 시스템, AP에 접속하지 않은 어쩌구~
- Sink: 센서 네트워크에서 센서 노드들의 센싱 데이터를 수집하는 노드
- WPAN(Wireless Personal Area Network): 스마트홈 기술, UWB, ZigBee, 블루투스 등
- DNS 서버 레코드 형식
    - A: 32비트 IPv4 주소에 연결
    - AAAA: 128비트 IPv6 주소에 연결
    - CNAME: 별칭, 가상 도메인 이름
- NS: 네임 서버, 특정 도메인의 DNS 정보를 관리하는 서버
- Windows Server IIS 관리자: 
- $ man ls: 'ls' 명령어 매뉴얼 출력
- NTFS
    - 퍼미션 사용 가능, 접근 권한을 사용자 별로 설정
    - 파일 시스템 암호화 지원
- ReFS: 데이터 오류를 자동으로 확인하고 수정(NTFS 기반)
- Windows 기본 계정: Administrator, DefaultAccount, Guest
- root: 리눅스 계정
- FTP: 20번 포트는 데이터 전송, 21번 포트는 제어용
- $ ifconfig | renew : 새로운 IP 주소 할당 요청
- DCHP: 가변 IP
- NAT: 사설 IP
- 리피터(Repeater): 물리계층(1계층) 장비
- RAID : 회전 패리티, 패리티 비트
- RAID 6: 이중 패리티
- RAID 10: RAID 1 + RAID 0
- RIPv1: 브로드캐스트
- RIPv2: 멀티캐스트
- runlevel 0: 종료
- runlevel 3: 다중사용자모드
- runlevel 5: 다중사용자모드, GUI
- runlevel 6: 재부팅

### 2022.11.06

- OSPF: 링크 상태(Link State) 라우팅 프로토콜, 동적 라우팅 프로토콜
- 유니캐스트: 단순 일대일
- 애니캐스트: 최근접 일대일
- 브로드캐스트: 단일송신-전체수신
- 멀티캐스트: 단일송신-다중수신
- Checksum: TCP 에러제어 헤더
- IGMP: TCP/IP 기반, Multicast
- 클라우드 컴퓨팅 시스템
    - SaaS: 소프트웨어 애플리케이션
    - PaaS: 개발 플랫폼
    - IaaS: 가상화 컴퓨팅 리소스
    - BPaaS: 백업 및 재해 복구 솔루션
- Multiplexing: 하나의 클라이언트가 여러 회선으로 수신 후 송신측에서 다시 여러 회선으로 나눔
- SDN(Software Defined Network): 소프트웨어 정의 네트워크, 라우터나 스위치 같은 네트워크 기기의 구성이나 연결 경로 등을 물리적인 기기의 도입과 배선 작업 없이 소프트웨어 설정 만으로 구현할 수 있는 기술
- PSDN(Public Switched Data Network): 공공교환데이터네트워크
- VPN(Virtual Private Network): 가상 전용 네트워크
- 저장소 복제: Windows Server에서 데이터 손실없이 동기 복제 제공, 데이터 손실 없이 백업 가능
- /etc/fstab: file system table
- ARP: Host(A) 레코드
- RARP: Pointer(PTR) 레코드
- $ nslookup {domain}: 도메인 호스트의 IP 주소 검색
- DFS(Distributed File System): 분산 파일 시스템
- ACK 필드 -> TCP 헤더에 포함

### 2022.08.21

- 버스 토폴로지: 터미네이터가 시그널의 반사를 방지하게 위해 사용됨
- IIS(Internet Information Services): 인터넷 정보 서비스, FTP를 구축, 운영하기 위해 먼저 설치되어 있어야 하는 서버
