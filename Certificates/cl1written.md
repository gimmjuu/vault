---
created:
    2024-08-30
tags:
    - CERTIFICATE
---

<link rel="stylesheet" href="../Assets/style.css" />
<style>
    h3 { padding: 7px 10px 8px; background-color: rgba(175, 239, 255, 0.05); border-radius: 3px; border: none; }
</style>

# Computer Literacy

- Computer literacy level 1

## 1과목 컴퓨터 일반

* 한글 Windows 10의 특징
    - 선점형 멀티태스킹
        + 앱 실행 중 문제 발생 시 해당 앱 강제 종료
        + 모든 시스템 자원 반환
    - 최대 255자 파일 이름 지정
* NTFS
    - FAT 파일 시스템보다 성능, 보안, 안정성, 속도면에서 뛰어남
        + 최대 크기 256TB
        + 파일 크기 최대 16TB(파일 크기는 볼륨 크기에 의해서만 제한)
* 바로가기 키(단축키)
    - F5: 새로고침
    - Alt + Esc: 현재 실행 중인 앱 전환(순서대로)
    - Alt + Enter: 등록 정보 표시
    - Ctrl + A: 폴더/파일 모두 선택
    - Ctrl + Shift + Esc: 작업 관리자 대화 상자 표시(= Ctrl + Alt + Del)
    - Ctrl + D: 삭제하기 (휴지통)
    - Shift + Delete: 바로 삭제 (휴지통 X)
    - Ctrl + F4: 파일 탐색기 (미리보기)
    - Alt + Shift + P: 파일 세부 정보 표시 (파일 탐색기)
    - 윈도우 + E: 파일 탐색기 실행
    - 윈도우 + L: 컴퓨터를 잠그거나 사용자(계정) 전환
    - 윈도우 + R: 실행 창
    - 윈도우 + U: 접근성 창
    - 윈도우 + Shift + S: 원하는 영역 캡처
* 바로가기 아이콘 (단축 아이콘)
    - 모든 개체에 대해 만들 수 있음
        + 바로가기 아이콘을 삭제해도 원본 파일은 삭제되지 않음
* 작업 표시줄
    - 위치 변경, 크기 조절 가능 (최대 크기는 화면의 1/2)
* 파일 탐색기 구성 요소: 라이브러리
    - 가상의 폴더: 폴더나 파일을 제거하면 원본 삭제
* 파일과 폴더
    - 연속적 항목 선택: Shift 누른 상태로 마지막 항목 클릭 (또는 드래그)
    - 비연속적 항목 선택: Ctrl 누르고 차례대로 클릭
    - 전체 항목 선택: Ctrl + A
    - 복사 및 이동
    - 색인 옵션 기능: 더 빠른 속도의 검색이 가능
        + 검색할 방법, 색인 위치, 파일 형식 조정

<details>
  <summary class="_yellowsmall">복사 및 이동?</summary>

| |복사|이동|
|:-:|:-:|:-:|
|C: → C: \n (같은 드라이브)|Ctrl + 드래그|드래그 앤 드롭|
|C: → D: \n (다른 드라이브)|드래그 앤 드롭|Shift + 드래그|
</details>

* 휴지통 ★★★
    - 삭제된 파일/폴더의 임시 보관 장소 → 복원이 가능함
    - 기본 크기: 드라이브 용량의 5-10%
    - 용량 초과 시 가장 오래 전 삭제 파일부터 자동으로 지워짐 (최근 파일 X)

<details>
  <summary>휴지통에 보관되지 않는 경우: 완전 삭제</summary>
* DOS 모드, 네트워크 드라이브, USB 메모리에서 삭제
* Shift + Delete
* 휴지통 속성에서 '휴지통에 버리지 않고 바로 제거'
* 휴지통 속성: 최대 크기 0MB 지정 (0%)
* 같은 이름의 항목을 복사/이동으로 덮어쓴 경우
</details>

* Windows 보조프로그램: 메모장
    - 간단한 텍스트(ASCII 형식) 파일, 문서 작성 앱
        + 문서 전체에 대해서만 글꼴 종류, 속성, 크기 변경
    - 그림, 차트 등 OLE 개체 삽입 X
    - Ctrl + G: 특정 줄로 이동
    - 문서의 첫 행 맨 왼쪽에 대문자로 .LOG 입력
        + 메모장을 열 때마다 현재의 시간, 날짜 표시 (문서의 끝에)
* 설정 → 시스템
    - 저장소: 하드디스크에서 불필요한 앱, 임시 파일, 휴지통 파일을 제거하여 사용 공간 확보 (자동)
* 설정 → 개인 설정
    - 배경: 고대비 설정
    - 글꼴: OTF, TTC, TTF, FON 등 확장자 파일 
        + 설치 폴더 위치: C:/Windows/Fonts
        + 설치: 글꼴 복사 후 붙여넣기
* 설정 → 접근성
    - 장애 / 컴퓨터에 익숙하지 않은 사람들을 위해 편리성을 제공
    - 고대비: 텍스트, 앱을 뚜렷하게 표시함
* 설정 → 업데이트 및 보안
    - Windows 보안: 컴퓨터 보호 (감염 시 치료)
        + 계정, 방화벽 및 네트워크 보호 / 장치 보안
* 프린트
    - 로컬 프린터: 컴퓨터에 직접 연결된 프린터
    - 네트워크 프린터: 다른 컴퓨터에 연결된 프린터 (공유 가능)
    - 기본 프린터: 특정 프린터를 지정하지 않을 경우 자동으로 인쇄 작업 전달 (*하나*만 지정 가능)
* *Spool* 스풀 기능
    - 저속 출력 장치인 프린터를 고속의 중앙처리장치(CPU)와 병행 처리할 때, 컴퓨터 전체의 처리 효율을 높임 (속도는 빨라지지 않음! 사용 시 인쇄 속도 느려짐)
* 드라이브 조각모음 및 최적화
    - 드라이브에 대한 *접근 속도를 향상*시키기 위함
* *명령 프롬프트*
    - MS-DOS 운영체제용 프로그램
    - 실행 방법
        1. 시작 → Windows 시스템 → 명령 프롬프트 선택
        2. 실행 창에 cmd 입력 → Enter
    - 종료: 명령 프롬프트 상 Exit 입력 후 Enter/닫기
* 레지스트리(Registry)
    - 컴퓨터에 설치된 모든 하드웨어(HW), 소프트웨어(SW)의 실행 정보를 한 군데에 모아 관리, 계층적인 데이터베이스 (추가/편집/백업/복원)
    - 레지스트리 편집기를 사용해 레지스트리를 잘못 변경하면 시스템을 손상시킬 수 있음 (변경 시 백업 필수!)
    - 수정: *REGEDIT* (레지스트리 편집 앱)
    - 관련 내용 저장: C:/Windows/System32/config
* 네트워크 및 인터넷
    - 이더넷: 현재 연결되어 있는 네트워크
    - 데이터 사용량
    - 백그라운드 데이터
        + 백그라운드에서의 수행 여부 설정 (사용량을 줄이기 위해!)
* TCP/IP 인터넷 표준 프로토콜 구성 요소
    - IP 주소: 컴퓨터 고유(유일) 주소
    - 서브넷 마스크: IPv4 주소의 네트워크 주소와 호스트 주소 구별
        + C 클래스: 255.255.255.0
    - 게이트웨이: 서로 다른 전송 프로토콜 간 변환 담당
    - DHCP 서버: 프로토콜, 동적으로 IP 주소 부여 *가변IP*
    - Ping: 원격 컴퓨터가 현재 네트워크에 연결, 정상적으로 작동하고 있는지 알아보는 서비스
    - ipconfig: 컴퓨터의 물리적(MAC) 주소, IP 주소, 서브넷 마스크, 게이트웨이 등을 표시
    - Tracert: 상대방 컴퓨터까지 연결되는 경로 표시 (→ Trace route)
* 문제 해결
    - 하드디스크 용량: 백업 후 불필요한 파일 삭제
    - 메모리 용량: 불필요한 앱(프로그램) 종료, 가상 메모리 크기 조절
        + 제일 좋은 방법은 메모리(RAM) 추가, 설치
* 펌웨어(Firmware) ★★★
    - 하드웨어 동작을 지시하는 소프트웨어
    - 하드웨어 교체없이 소프트웨어 업그레이드만으로 시스템 성능 향상 가능
    - 주로 *ROM*에 저장 / 하드웨어 제어 및 관리

|디지털 컴퓨터|아날로그 컴퓨터|
|:-:|:-:|
|숫자, 문자 (가산) → 비연속적|전류, 전압, 온도 (불가산) → 연속적인 자료|
|논리 회로|증폭 회로|
|프로그래밍 필요 (설치)|불필요 (미리 설치됨)|
|범용|특수 목적용|

* 하이브리드 컴퓨터
    - 디지털 컴퓨터와 아날로그 컴퓨터의 장점을 혼합
* 컴퓨터의 계산 속도 단위
    - 저속 → 고속
    - ms(milli second) → μs(micro second) → ns(nano second) → ps(pico second) → fs(femto second) → as(atto second)
    - 밀리-마이크로-나노-피코-펨토-아토
* 자료 구성의 단위
    - 비트(bit): 자료 표현의 최소 단위(니블: 4비트)
    - 바이트(Byte): 문자 표현, 8개의 비트가 모여 1Byte
    - 워드(Word): 단어 표현
    - 필드(Field): 특정 항목
    - 레코드(Record): 자료
    - 파일(File): 여러 레코드가 모여서 구성
    - 데이터베이스(Database): 파일 집합
* 진법 변환 ★★★★★
    - 10진수를 2진수, 8진수, 16진수로 변환
        + 정수 부분: 나눠지지 않을 때까지 나누고, 몫을 제외한 나머지를 역순으로 표시 (밑에서 위로 취합)
        + 소수 부분: 10진수 값에 변환할 진수를 곱한 후 결과의 정수 부분만 차례대로 표기 (0 또는 반복되는 수가 나올 때까지 곱하기 반복)
    - 2진수, 8진수, 16진수를 10진수로 변환
        + 정수 부분과 소수 부분을 나누어 변환하려는 각 진수의 자리 값과 자리의 지수 승을 곱한 결과값을 *모두 더하여* 계산
    - 2진수를 8진수로
        + 정수 부분은 왼쪽 방향으로 3자리씩, 소수 부분은 오른쪽 방향으로 3자리씩(421코드)
    - 2진수를 16진수로
        + 정수 부분은 왼쪽 방향으로 4자리씩, 소수 부분은 오른쪽 방향으로 4자리씩 (8421)
    - 8진수, 16진수를 2진수로
        + 8진수 1비트는 2진수 3비트로, 16진수 1비트는 2진수 4비트로 풀어 변환

|16진수|표기|
|:-:|:-:|
|10|A|
|11|B|
|12|C|
|13|D|
|14|E|
|15|F|

* 문자 표현 코드 ★★★
    - BCD 코드 (2진화 10진): 6비트, 64가지 문자 표현
        + 영문 소문자 표현 x
    - ASCII 코드: 7비트, 128가지 표현, 데이터 통신용
        + 확장 ASCII는 8비트 사용 - 256가지 문자
    - EBCDIC 코드: BCD 코드 확장, 8비트, 256가지 표현
    - 유니코드: 국제 표준 코드(모든 문자 2바이트로 표현) 16비트(2바이트), 최대 65,536자의 글자 코드화
* 에러 검출 코드
    - 패러티 체크 비트: 에러 검출
    - 해밍 코드: 에러 검출 및 교정 기능
    - 순환 중복 검사(CRC): 오류 검출 우수함
    - 블록합 검사(BSC): 패리티 검사의 단점 보완
* 중앙처리장치(CPU)
    - 제어장치-연산장치-*레지스터*로 구성
    - 제어장치(CU) ★
        + 프로그램 카운터(PC): 다음에 실행할 명령어의 번지 기억
        + 명령 레지스터(IR): 현재 실행 중인 명령 내용 기억
        + 명령 해독기(Decoder): 명령어 해독 (10진수로 변환)
        + 부호기(Encoder): 해독된 명령에 따라 제어 신호 생성 (2진수)
    - 연산장치(ALU)
        + 가산기(Adder): 2진수 덧셈 수행 (회로)
        + 보수기(Complementor): 뺄셈을 위해 입력된 값을 보수로 변환 (회로)
        + 누산기(Accumulator): 연산 결과 일시적 저장
* 레지스터(Register)
    - CPU 내부에서 처리할 명령어, 연산의 중간값 등을 일시적으로 저장하는 기억장치
    - 일반적으로 플립플롭(Flip-Flop)이나 래치(Latch) 등을 연결하여 구성 (메모리 중 속도가 가장 빠름)
    - 크기: 컴퓨터가 한번에 처리할 수 있는 데이터의 크기를 나타냄
* ROM: 비휘발성 메모리 ★★★
    - 입·출력 시스템(BIOS), 글자 폰트, 자가 진단 프로그램(POST) 저장
* RAM: 휘발성 메모리
    - 사용 중인 프로그램, 데이터 저장(자유로운 읽고 쓰기)

|구분|동적 램(DRAM)|정적 램(SRAM)|
|:-:|:-:|:-:|
|재충전|필요|필요x|
|전력 소모|적음|많음|
|접근 속도|느림|빠름|
|가격|저가|고가|
|용도|주기억장치|캐시메모리|

* 기타 메모리
    - 캐시 메모리: CPU와 주기억장치 사이에서 컴퓨터의 처리 속도 향상 (속도가 빠른 SRAM 사용)
    - 가상 메모리: 보조기억장치(하드디스크)의 일부를 주기억장치처럼 사용
    - 버퍼 메모리: 장치 간 데이터를 주고 받을 때 속도 차이 해결을 위한 임시 저장 공간
    - 연관(연상) 메모리: 저장된 내용의 일부를 이용해 기억장치에 접근하여 데이터를 읽어오는 장치
    - 플래시 메모리: EEPROM의 일종, MP3 플레이어, 개인용 정보 단말기, 휴대전화, 디지털 카메라에 사용
* 보조기억장치
    - 비휘발성, 속도가 느림, 대용량, 단위당 가격이 저렴
    - SSD
        + 반도체 이용, 배드섹터 발생 X, 발열/소음/전력 소모 적음, 소형화 및 경량화 가능, 외부 충격에 강하나 저장 용량당 가격이 비쌈 (부팅 속도 빠름)
    - Blu-ray
        + 고선명(HD) 비디오, DVD에 비해 약 10배 이상의 데이터(단층 25GB, 복층 50GB) 저장, DVD를 블루레이에서 사용 불가(반대는 가능)
    - 픽셀(화소)
        + 모니터 화면 구성의 가장 작은 단위
    - 해상도(선명도)
        + 픽셀 수에 따라 결정됨
    - 플리커프리
        + 모니터의 깜빡임 현상인 플리커를 제거해 눈의 피로, 두통 등의 증상을 줄려주는 기술
* 인터럽트(Interrupt)
    - 프로그램 실행 도중 예기치 않은 상황 발생 시 현재 작업 일시 중단, 발생 상황을 우선 처리 후 실행 중인 작업으로 복귀하여 계속 처리하는 것
* 채널(Channel)
    - CPU 대신 입·출력을 관리 (제어 권한을 넘겨받음)
* 마이크로프로세서

|구분|RISC|CISC|
|:-:|:-:|:-:|
|명령어|적음|많음|
|주소 지정|간단|복잡|
|레지스터|많음|적음|
|전력 소모|적음|많음|
|처리 속도|빠름|느림|
|용도|서버, 워크스테이션|개인용 컴퓨터|

* 포트
    - 직렬: 한 비트씩 전송
    - 병렬: 8 비트씩 전송 (직렬보다 빠름)
    - USB(범용 직렬 버스): 최대 127개까지 연결
    - IEEE 1394: 가전기기를 컴퓨터에 연결
    - IrDA: 케이블없이 (무선) 적외선 사용
    - HDMI: 하나의 케이블로 전송하는 디지털 포트
* 바이오스(BIOS)
    - 전원이 켜지면 POST(Power On Self)를 통해 컴퓨터 점검 후 사용 가능한 장치 초기화
        + 윈도우가 시작될 때까지 부팅 과정을 이끔
    - *CMOS에서 설정 가능한 항목*
        + 시스템의 날짜와 시간
        + 하드디스크 타입
        + 부팅 순서
        + 칩셋
        + 전원 관리
        + PnP
        + 시스템 암호
        + Anti-Virus 등
        + ***윈도우 로그인 암호*** (x)
* 하드디스크 연결 방식
    - IDE: 2개 (장치 연결 가능)
    - EIDE(ATA): 4개
        + 종류: PATA, SATA / *EIDE는 일반적으로 PATA*
    - SCSI: 7개
* RAID (Redundant Array of Inexpensive Disk)
    - 여러 개의 하드디스크를 한 개의 하드디스크처럼 관리하는 기술 (미러링, 스트라이핑 기술을 융합해 사용)
    - 미러링 방식
        + 데이터를 두 개의 디스크에 동일하게 기록
        + 한쪽 디스크 데이터 손상 시 다른 한쪽 디스크를 이용하여 복구
    - 스트라이핑 방식
        + 데이터를 여러 개의 디스크에 나눠 기록
        + 시간 단축은 가능, 디스크가 한 개라도 손상되면 데이터 사용 불가
* 업그레이드
    - CPU 업그레이드
        + 시스템의 성능을 향상시킬 수 있는 가장 확실한 방법
        + 주로 메인보드와 함께 교체해 등급을 높임
    - 고려사항?

|수치가 클수록 좋은 것|수치가 작을수록 좋은 것|
|:-:|:-:|
|CPU 클럭 속도, CPU 성능, 모뎀 전송 속도, CD-ROM 드라이브 전송 속도, 하드디스크 용량, 하드디스크 회전 수, 하드디스크 전송 속도|RAM 접근 속도(ns)|

* 파티션 (Patition)
    - 하나의 물리적인 하드디스크를 여러 개의 논리적인 영역으로 나누는 작업 (기본/확장 파티션)
    - 목적
        + 특정 데이터만 별도로 보관할 드라이브 확보
        + 하나의 하드디스크에 서로 다른 운영체제 설치
    - 하나의 파티션에 한 개의 파일 시스템만 사용 가능하며 파티션 설정 후 데이터 저장을 위해 포맷 과정을 거쳐야함
* 사용권에 따른 소프트웨어 분류
    - 상용 소프트웨어
        + 정식으로 대가를 지불하고 사용하는 프로그램
    - 셰어웨어
        + 기능 혹은 사용 기간에 제한을 두어 배포(무료, 정식 프로그램의 구입을 유도하기 위해)
    - 프리웨어
        + 무료로 사용, 배포 가능
    - 공개 소프트웨어
        + 소스 공개, 누구나 자유롭게 사용, 수정, 재배포 가능(무료)
    - 데모 버전
        + 사용 기간, 기능을 제한해 배포 (홍보 목적)
    - 알파 버전
        + 베타테스트 전, 제작 회사 내 테스트 목적
    - 베타 버전
        + 정식 프로그램 출시 전, 테스트 목적으로 일반인에게 공개
    - 패치 버전
        + 제작하여 배포된 프로그램의 오류 수정, 성능 향상을 위해 일부 파일을 변경하여 재배포
    - 애드웨어
        + 프리웨어, 셰어웨어 등에서 광고를 보는 대가로 사용이 허용
    - 번들
        + 특정 하드웨어, 소프트웨어 구입 시 무료로 끼워주는 소프트웨어
    - 트라이얼 버전
        + 체험판 프로그램
    - 오픈소스 프로그램
        + 소스 공개 (수정, 변경 가능)
* 운영체제(OS)
    - 가장 대표적인 시스템 소프트웨어
    - 목적
        + 신뢰도 향상
        + 사용 가능도(시스템을 사용할 필요가 있을 때 즉시 사용 가능한 정도) 확대
        + 처리 능력 증대
        + 응답 시간 단축
    - 32비트용으로 설계된 프로그램은 64비트 윈도우에서 호환되는 게 많으나, 64비트 전용으로 만들어진 프로그램은 64비트 윈도우에서만 작동
* 운영체제의 운영 방식
    1. 일괄 처리
        + 처리할 데이터를 일정량, 일정 기간동안 모았다가 한꺼번에 처리 (1세대)
    2. 실시간 처리
        + 항공기, 열차 좌석 예약, 은행 (2세대)
    3. 다중 프로그래밍
        + *현대의 CPU*로 여러 개의 프로그램을 동시에 처리
    4. 다중 처리
        + 여러 개의 CPU를 설치해 프로그램 처리 (처리 속도 향상, 엄청 빠름)
    5. 분산 시스템
        + 여러 대의 컴퓨터를 연결해 작업 분담
    6. 임베디드 시스템
        + 마이크로프로세서에 특정 기능을 수행하는 응용 프로그램을 탑재하여 컴퓨터의 기능을 수행 (디지털TV, 전기밥솥, 냉장고, PDA 등)
    7. 듀얼 시스템
        + 두 개의 컴퓨터가 같은 업무를 동시에 처리
        + 한쪽 컴퓨터가 고장나면 다른 컴퓨터가 계속 업무를 처리하여 업무 중단 방지
    8. 클러스터링
        + 두 대 이상의 컴퓨터를 함께 묶어 단일 시스템처럼 사용하는 기술
* 주요 고급 언어의 특징
    - JAVA
        + 객체 지향 언어 (플랫폼에 관계없이 독립적)
        + 운영체제 및 하드웨어에 독립적 -> 이식성 Good!
    - C
        + UNIX 운영 체제 제작을 위해 개발
        + 저급-고급 언어의 특징을 고루 갖춘 중급 언어
    - LISP
        + 인공지능 분야에 사용되는 언어
        + 재귀 호출
    - C++
        + C언어에 객체 지향 개념을 적용한 언어
* 객체 지향 프로그램
    - 유지보수가 쉽고 코드의 재사용이 가능한 프로그램 생성 (객체 중심)
    - 특징
        + 상속성
        + 캡슐화(은닉성)
        + 추상화
        + 다형성
        + 오버로딩
        + 계층성
        + 모듈성
* 언어 번역 과정
    - 원시 프로그램 -> 컴파일러 -> 목적 프로그램 -> 로드 모듈 -> 실행
    - 번역
        + 컴파일러와 같은 언어 번역 프로그램 사용
        + 기계어로 변환, 목적 프로그램 생성
    - 링커
        + 하나의 실행 가능한 모듈(실행 파일)로 만듦
    - 로더
        + 실행 가능한 로드 모듈에 기억 공간의 번지 지정, 메모리에 적재
        + 기능: 할당, 연결, 재배치, 적재
* 웹 프로그래밍 언어
    - ASP/JSP/PHP
        + 서버 측, 스크립트 언어
* 네트워크 운영 방식 ★
    - 중앙 집중(Host-Terminal) 방식
        + 모든 처리를 담당하는 중앙 컴퓨터와 데이터의 입,출력 기능을 담당하는 단말기로 구성
        + 포인트 투 포인트 방식: 중앙 컴퓨터와 단말기를 1:1 독립적으로 연결 (언제든지 데이터 전송 가능)
    - 클라이언트/서버(Client/Server) 방식
        + 정보를 제공하는 서버, 정보 요구하는 클라이언트
        + 서버와 클라이언트 모두 처리 능력을 가지고 있어 분산 처리 환경에 적합함
    - 동배간(Peer-To-Peer) 처리 방식
        + 모든 컴퓨터를 동등하게 연결하는 방식 (p2p)
        + 시스템에 소속된 컴퓨터들은 어느 것이든 서버가 될 수 있으며, 동시에 클라이언트도 될 수 있음
* 망의 구성 형태
    - 성형(Star, 중앙 집중형)
        + 모든 노드가 중앙 노드에 1:1 연결
        + 처리 능력 및 신뢰성은 중앙 노드의 제어 장치에 의해 좌우, 고장 발견이 쉽고 유지 보수 및 확장 용이
    - 링형(Ring, 루프형)
        + 하나라도 고장 나면 전체 통신망에 영향
        + 단말장치의 추가/제거 및 기밀 보호가 어려옴
    - 버스형(Bus)
        + 회선 양끝에는 종단장치 필요, 설치 및 제거 용이 (고장나도 통신망 전체에 영향X 신뢰성O)
        + 기밀 보장 어려움, 통신 회선 길이에 제한 O
    - 계층형(Tree, 분산형)
        + 중앙 컴퓨터와 일정 지역의 단말장치까지는 하나의 통신 회선으로 연결
        + 회선의 확장이 많을 경우 트래픽이 과중 될 수 있음
    - 망형(Mesh)
        + 응답시간이 빠르고 많은 양의 통신 필요시 유리함
        + 필요한 회선수: N(N-1)/2 -> n=노드
* 네트워크 관련 장비
    - 허브 Hub
        + 네트워크를 구성할 때 한꺼번에 여러 대의 컴퓨터를 연결 (각 회선 통합 관리)
    - 리피터 Repeater
        + 장거리 전송을 위해 수신한 신호 재생 혹은 출력 전압을 높여 전송하는 장치 (증폭기)
    - 브리지(Bridge)
        + 물리적으로 다른 네트워크를 연결할 때 사용
    - 라우터(Router)
        + 최적의 경로를 설정해 전송, 데이터 흐름 제어
    - 게이트웨이(Gateway)
        + 데이터를 받아들이는 출입구 역할
* 인트라넷 Intranet
    - 인터넷의 기술을 기업 내 정보 시스템에 적용 (전자우편, 결재 시스템 등)
* 엑스트라넷 Extranet
    - 기업과 기업 간에 인트라넷을 연결
    - 기업체와 원활한 통신을 위해 인트라넷의 이용 범위를 확대함
* IP 주소 (IPv4)
    - 인터넷 주소로 8비트씩 4부분 총 32비트
    - 네트워크 부분의 길이에 따라 A클래스에서 E클래스까지 5단계 구성

|클래스|타입|
|:-:|:-:|
|A Class|국가, 대형 통신망 [255.0.0.0]|
|B Class|중대형 통신망 [255.255.0.0]|
|C Class|소규모 통신망 [255.255.255.0|
|D Class|멀티캐스트용|
|E Class|실험용|

* IPv6
    - 16비트씩 8부분 총 128비트
    - 포화 상태에 있는 IPv4 대체 (차세대 주소 체계)
    - 4가지 16진수를 콜론(:)으로 구분
    - 인증성, 기밀성, 데이터 무결성 (+IPv4보다 빠름)
    - 유니캐스트, 멀티캐스트, 애니캐스트의 3가지 주소 체계로 분류
* 도메인 네임
    - IP 주소(숫자)를 이해하기 쉬운 문자 형태로 표현
    - DNS(Domain Name System)
        + 문자로 된 도메인 네임을 숫자로 된 IP 주소로 바꿔주는 시스템
* URL (Uniform Resource Locater)
    - 인터넷 상에 존재하는 각종 자원이 있는 위치를 나타내는 표준 주소 체계
    - 형식
        + 프로토콜://호스트(서버) 주소[:포트 번호][/파일 경로]
    - 포트 번호
        + News: 119
        + HTTP: 80
        + TELNET: 23
        + FTP: 21
        + GOPHER(정보 검색): 70
* 프로토콜 ★
    - 네트워크에서 서로 다른 컴퓨터 간에 정보 교환을 하도록 하는 통신 규약(규칙)
    - 통신망에 흐르는 패킷 수를 조절(흐름 제어)
    - 전체의 안전성 유지
    - 동기화 기능
    - 프로토콜의 종류
        + TCP: 패킷 단위로 정보 분류, 흐름 제어+에러 유무 검사 (전송 계층)
        + IP: 패킷 주소 해석+ 경로 결정 (네트워크 계층)
        + ARP: IP 주소를 이용해 물리적인 MAC 주소를 변경 (←→ RARP)
* OSI 7계층
    - 물리 계층
    - 데이터링크 계층
    - 네트워크 계층
    - 전송 계층
    - 세션 계층
    - 표현 계층
    - 응용 계층
* OSI 7계층과 관련된 네트워크 장비
    - 물리 계층: 리피터, 허브
    - 데이터링크 계층: 랜카드, 브릿지 스위치
    - 네트워크 계층: 라우터
    - 전송 계층: 게이트웨이
* 전자우편
    - 기본적으로 7Bit의 ASCII 코드를 사용해 메시지를 주고 받음
        + 사용자_ID@메일_서버_주소
    - 전자우편 프로토콜
        + SMTP: 작성된 메일을 전송
        + POP3: 수신, E-MAIL을 가져옴
        + MIME: 멀티미디어 파일 내용 확인, 실행
        + IMAP: 전자우편 엑세스, 이메일 제목만 다운로드
* FTP 파일 전송 프로토콜
    - 기본 포트번호: 21번
        + 다른 번호로 변경 가능
    - *Anonymous FTP*: 계정이 없는 사용자도 접근해 사용할 수 있는 서비스 (익명)
    - FTP 서버에 있는 프로그램은 다운로드를 해야 사용 가능
        + 서버에서 바로 실행 불가능
    - *Binary* 모드: 그림 전송
    - *ASCII* 모드: 텍스트 파일 전송
* 기타 인터넷 서비스
    - WWW
        + HTTP 프로토콜을 사용, 하이퍼텍스트 기반
    - 텔넷
        + 멀리 털어져 있는 컴퓨터에 접속, 자신의 컴퓨터처럼 사용할 수 있도록 하는 서비스
    - IRC
        + 인터넷상에서 채팅 가능
    - TRACERT
        + 인터넷 서버까지 경로를 추적하는 명령어
    - Nslookup (Name Server lookup)
        + 도메인 네임 서버 검색 서비스 → 도메인 네임을 이용해 IP 주소 찾기
* ICT 신기술 관련 용어
    - 그리드 컴퓨팅
        + 수많은 컴퓨터를 하나의 고성능 컴퓨터처럼 활용
    - 유비쿼터스 컴퓨팅
        + 관련 기술: RFID
    - 사물 인터넷 IoT
        + *인간*과 사물, 사물과 사물 간 언제 어디서나 서로 소통할 수 있게 하는 새로운 정보 통신 환경
    - 테더링 (Tethering)
        + 인터넷에 연결된 기기를 이용해 다른 기기도 인터넷 사용이 가능하도록 해주는 기술
        + 노트북과 같은 IT 기기를 휴대폰에 연결해 무선 인터넷 사용 가능
    - 텔레매틱스 (Telematics)
        + 자동차에 정보 통신 기술과 정보 처리 기술을 융합해 운전자에게 다양한 멀티미디어 서비스를 제공
    - 유비쿼터스 센서 네트워크의 활용 분야에 속함
    - Wibro (와이브로)
        + 무선 광대역
        + 언제 어디서나 이동하면서 고속으로 무선 인터넷 접속이 가능한 서비스
    - 데이터 마이닝 (Data Mining)
        + 통계 기법, 인공지능 등을 이용해 대량의 데이터에서 유용한 정보를 추출
* 멀티미디어
    - 매체(텍스트, 그래픽, 사운드)를 디지털로 통합, 전달
    - 특징
        + 디지털화, 쌍방향성, 비선형성, 정보의 통합성
    - 순차적 X (비순차적: 비선형) → 다양한 방향으로 처리
* 그래픽 기법
    - 디더링
        + 제한된 색상을 조합해 복잡/새로운 색을 만듦
    - 렌더링
        + 3차원 애니메이션을 만드는 과정 중 하나
        + 물체의 모형에 명암과 색상을 입혀 사실감을 더함
    - 모델링
        + 렌더링을 하기 전에 수행되는 작업
        + 물체의 형상을 3차원 그래픽으로 어떻게 표현할지 결정
    - 모핑
        + 2개의 이미지를 부드럽게 연결해 변환/통합
    - 필터링
        + 이미 작성된 그림을 필터 기능을 이용해 여러가지 태의 새로운 이미지로 바꿔주는 작업
    - 리터칭
        + 이미지를 다른 형태로, 새롭게 변형/수정
    - 안티앨리어싱
        + 이미지의 가장자리가 톱니 모양(계단 현상)을 없애기 위해 경계선을 부드럽게, 필터링 기술
    - 인터레이싱
        + 이미지의 대략적인 모습을 먼저 보여준 다음 점차 자세한 모습을 보여주는 기법
* 그래픽 데이터의 표현 방식 ★★★

|그래픽 데이터|특징|
|:-:|:-|
|비트맵<br>(Bitmap)|● 점(Pixel, 화소)으로 이미지 표현 (래스터 이미지)<br>● 화면 표시 속도는 빠르지만 이미지를 확대하면 테두리가 거칠게 표현됨(계단 현상) → 안티앨리어싱 처리 필요<br>● 사실적인 이미지 표현<br>● 벡터 방식에 비해 많은 메모리 차지<br>● 파일 형식: BMP, TIF, GIF, JPEG, PCX, PNG 등<br>● 프로그램: 그림판, 포토샵, 페인트샵|
|벡터<br>(Vector)|● 점과 점을 연결하는 직선/곡선을 이용해 이미지 표현<br>● 이미지를 확대해도 테두리가 거칠어지지 않고 매끄럽게 표현<br>● 단순한 도형과 같은 개체 표현 적합<br>● 파일 형식: DXF, AI, WMF 등<br>● 프로그램: 일러스트레이터, 코렐드로우, 플래시 등|

* 그래픽 파일 형식
    - GIF
        + 8비트 컬러 - 256가지 색 표현
        + 애니메이션 표현 가능 (무손실 압축 기법)
    - JPEG(JPG)
        + 24비트 컬러 - 16,777,216가지 색 표현
        + 손실 압축 기법과 무손실 압축 기법 사용
    - PNG
        + 인터넷에서 이미지 표현 최적
        + 부드러운 투명층 표현
* 오디오 데이터

|오디오 데이터|특징|
|:-:|:-|
|WAVE|실제 소리가 저장되어 재생이 쉽지만,<br>용량이 큼|
|MIDI|음성/효과음의 저장 불가능<br>시퀀싱 작업, 16개 이상 악기 동시 연주 가능|
|MP3|MPEG-1의 압축 방식, 음반 CD 음질을 유지하며 1/12까지 압축 가능|
|FLAC|무손실 압축 포맷, 음원 손실 거의 없음|
|AIFF|비압축 무손실 압축 포맷 (애플)|

* 비디오 데이터
    - AVI
        + Windows의 표준 동영상 파일 형식
        + 별도의 하드웨어 장치 없이 재생 가능
    - 퀵 타임 MOV
        + Apple 사에서 개발한 동영상 압축 기술
    - MPEG
        + 동영상+오디오 압축 가능
        + 압축률을 높이는 손실 압축 기법 (무손실 비지원)
* MPEG 규격
    - MPEG-2
        + MPEG-1의 화질 개선
        + HDTV 등
    - MPEG-4
        + MPEG-2의 압축률 개선
        + IMT-2000 환경에서 영상 정보 압축 전송 시 필수적 요소로 인정
    - MPEG-7
        + 멀티미디어 정보 검색이 가능한 동영상
        + 데이터 검색 및 전자상거래 등에 사용
    - *MPEG-21*
        + 위의 MPEG 기술들을 통합해 디지털 콘텐츠 제작, 유통, 보안 등 전 과정 관리가 가능한 기술
        + 멀티미디어 프레임워크의 MPEG 표준
* 바이러스의 감염 경로와 예방법
    - 공유 폴더의 속성을 *읽기 전용*으로 지정
        + 숨김 속성은 네트워크 감염 방지 불가!
    - 발신자가 불분명한 전자우편 열어보지 않고 바로 삭제
    - 중요한 자료는 정기적으로 백업
* 보안 위협의 형태 ★★★
    - 웜 (Worm)
        + 네트워크를 통해 연속적으로 자신을 복제하여 시스템의 부하를 높임
        + 결국 시스템을 다운시키는 바이러스
    - 트로이 목마
        + 정상적인 프로그램으로 가장해 프로그램 내에 숨어 있다가 해당 프로그램이 동작할 때 활성화되어 부작용을 일으킴
        + 자기 복제 능력 없음!
    - 백도어 (Back/Trap Door)
        + 서비스 기술자가 액세스 편의를 위해 만든 보안이 제거된 비밀통로
        + 시스템에 무단 접근하기 위한 일종의 비상구로 사용
    - 눈속임 (Spoof)
        + 어떤 프로그램이 정상적으로 실행되는 것처럼 속임수를 사용
    - 스니핑 (Sniffing)
        + 패킷을 엿보면서 계정과 패스워드 등의 정보를 가로채는 행위
    - 스푸핑 (Spoofing)
        + 검증된 사람이 네트워크를 통해 데이터를 보낸 것처럼 변조, 접속 시도
    - 피싱 (Phishing)
        + 거짓 메일을 발송해 금융기관의 정보 등을 빼내는 기법
    - 키로거 (Key Logger)
        + 키보드 상 키 입력 캐치 프로그램을 이용해 ID/암호 등 개인 정보를 빼내 악용
    - 크래킹/크래커
        + 시스템에 불법 침입해 정보를 파괴, 내용을 자신의 이익에 맞게 변경
    - *분산 서비스 거부 공격 (DDOS)*
        + 여러 대의 장비를 이용해 대량의 데이터를 한 곳의 서버에 집중적으로 전송하여 특정 서버의 정상적인 기능을 방해
* 방화벽 (Firewall) ★★★
    - 외부의 불법 침입으로부터 내부의 정보 자산을 보호
    - *역추적* 기능
        + 외부의 침입자의 흔적을 찾을 수 있음
        + 내부로부터 불법적인 해킹은 막을 수 없음 → 불완전한 보안
* 프록시 서버 (Proxy Server)
    - PC 사용자와 인터넷 사이에서 중계자 역할
    - 방화벽 기능과 캐시 기능이 있음
* 비밀키/공개키 암호화 기법

|기법|특징|
|:-:|:-|
|비밀키 암호화 기법|DES<br>동일한 키로 데이터를 암호화/복호화<br>(장점) 속도가 빠르고 알고리즘이 단순하며 파일 크기가 작음<br>(단점) 사용자의 증가에 따라 관리해야 할 키의 수가 많아짐|
|공개키 암호화 기법<br>(비대칭)|RSA<br>서로 다른 키로 암호화/복호화<br>(장점)관리할 키의 개수가 적음<br>(단점) 속도가 느리고 알고리즘이 복잡하며 파일 크기가 큼<br>→ 데이터를 암호화할 때 사용되는 키(공개키)는 공개/복호화할 때 사용하는 키(비밀키)는 비공개로 함|

## 2과목 스프레드시트 일반

* 워크시트 선택
    - 연속적인 여러 개의 시트 선택
        + Shift를 누른 채로 클릭
    - 비연속적인 여러 개의 시트 선택
        + Ctrl을 누른 채로 클릭
    - 워크시트 최대 255개 삽입 가능
        + Shift & F11
    - 삭제된 시트는 복구 불가능
    - 여러 개의 시트 한번에 삭제 가능
* 데이터 입력
    - 실행 취소: Ctrl + Z
    - 다시 실행: Ctrl + Y
* 메모
    - 오른쪽 상단에 빨간색 삼각형 점이 표시됨
    - 셀에 입력된 데이터를 지워도 메모는 삭제되지 않음
    - 셀 이동 시 메모도 함께 이동함
* 윗주 ★
    - 셀에 입력한 데이터 위쪽에 추가 가능
    - 문자 데이터만 삽입 가능
    - 윗주 서식은 윗주 전체에 대해서만 적용, 변경 가능
* 채우기 핸들을 이용한 연속 데이터 입력
    - 숫자 데이터
        + 1셀 드래그: 동일 데이터 복사
        + 1셀 Ctrl+드래그: 값 1씩 증가
        + 2셀 잡고 드래그: 첫번째 셀과 두번째 셀 값 차이만큼 증가/감소
    - 문자 데이터
        + 동일한 데이터 복사
    - 혼합 데이터 (숫자+문자)
        + 1셀 드래그: 가장 오른쪽에 있는 숫자만 1씩 증가
        + 2셀 드래그: 숫자 데이터 차이만큼 증가/감소
    - 날짜 데이터
        + 1셀 드래그: 1일 단위 증가
        + 2셀 드래그: 두 셀 간 차이만큼 연,월,일 단위 증가
* 찾기
    - Ctrl + F / Shift + F5
    - 역순 검색: Shift + [다음 찾기]
* 워크시트 전체
    - 범위 지정
        + Ctrl + A
        + Ctrl + Shift + Spacebar
* 통합 문서 공유
    - 데이터의 입력과 편집은 가능
    - 조건부 서식, 차트, 시나리오, 부분합, 데이터표, 피벗 테이블 보고서 등의 작업은 추가/변경 불가능
    - 엑셀의 상위 버전에서 작성된 공유 통합 문서는 하위 버전에서 사용 불가
* 시트 보호
    - 데이터나 차트 등을 변경할 수 없도록 보호
    - 특정 시트만 보호 (나머지 시트는 변경 가능)
* 검토 → 변경 내용 → 통합 문서 보호
    - 이름 바꾸기 등을 할 수 없도록 보호 (암호 지정 가능)
* 사용자 지정 서식 ★
    - 양수, 음수, 0, 텍스트 순으로 표시 → *;* 구분
    - `#,###;[빨강](#,###) ; 0.00 ; @”님”`
    - 조건 또는 글꼴색 지정
        + 대괄호(*[]*) 안에 입력
    - *#*: 유효하지 않은(불필요한) 0은 표시하지 않음
    - *0*: 유효하지 않은 자릿수를 0으로 표시
* 조건부 서식
    - 규칙에 만족하는 셀에만 셀 서식 적용
    - 조건부 서식의 규칙을 수식으로 입력할 경우 수식 앞에 반드시 등호(*=*)를 입력함
* 오류 메시지 ★
    - ####
        + 셀에 셀 너비보다 큰 숫자, 날짜, 시간이 있거나 셀에 계산 결과가 음수인 날짜와 시간이 있을 때
    - #DIV/0!
        + 나누는 수가 빈 셀이나 0이 있는 셀을 참조
    - #N/A
        + 함수나 수식에 사용할 수 없는 값을 지정
    - #NAME?
        + 인식할 수 없는 텍스트를 수식에 사용
    - #NULL!
        + 교차하지 않는 두 영역의 교점을 지정
    - #NUM!
        + 표현할 수 있는 숫자의 범위를 벗어났을 때
    - #REF!
        + 셀 참조가 유효하지 않을 때
    - #VALUE!
        + 잘못된 인수나 피연산자 사용 / 수식 자동 고침 기능으로 수식을 고칠 수 없을 때
* 셀 참조
    - 상대 참조: A1
    - 절대 참조: $A$1
    - 열 고정: $A1
    - 행 고정: A$1
* 이름 작성 규칙
    - 첫 문자는 반드시 문자(영문, 한글)나 밑줄(_) 또는 역슬래시(\\)로 시작해야 함
    - 이름에 공백 포함 *불가*
    - 최대 255자까지 지정 가능
* 통계 함수
    - 
* 수학/삼각 함수 ★
    -
* 텍스트 함수
    - 
    - FIND: 대소문자 구분, 와일드카드 문자 사용 불가
    - SEARCH: 대소문자 미구분, 와일드카드 문자 사용 가능
    - VALUE: 텍스트를 숫자로 변환
* 날짜/시간 함수
    - 
* 논리 함수
    - 
* 정보 함수
    - ISBLANK(인수): 빈 셀이면 참
    - ISERROR(인수): 오류 값을 가지고 있으면 참
    - ISERR(인수): N/A를 제외한 오류값을 가지고 있으면 참
    - ISEVEN(인수): 짝수면 참
    - ISODD(인수): 홀수면 참
* 찾기/참조 함수 ★
    - VLOOKUP(기준값, 범위, 열 번호, 옵션)
        + 범위의 첫번째 열에서 기준값과 같은 데이터를 찾은 후, 기준값이 있는 행에서 지정된 열 번호 위치에 있는 데이터 표시
    - HLOOKUP(기준값, 범위, 행 번호, 옵션)
        + 범위의 첫번째 행에서 기준값과 같은 데이터를 찾은 후, 깆ㄴ값이 있는 열에서 지정된 행 번호 위치에 있는 데이터 표시
    - CHOOSE(인수, 첫번째, 두번째, …)
        + 인수가 1이면 첫번째를, 인수가 2면 두번째를 입력
    - OFFSET(범위, 행, 열, 높이, 너비)
        + 선택한 범위에서 지정한 행과 열만큼 떨어진 위치에 있는 데이터 영역의 데이터 표시
    - COLUMN(셀)
        + 셀의 열번호
    - ROW(셀)
        + 셀의 행번호
* 데이터베이스 함수
    - DSUM
    - DAVERAGE
    - DCOUNT
    - DVAR
    - DGET
    - (제목 포함 전체 범위, 계산할 제목, 제목 포함 조건)
* 재무 함수
    - FV: 미래 가치
    - PV: 현재 가치
    - PMT: 정기 상환 금액
* 배열 수식
    - 사용되는 배열 인수
        + 동일한 개수의 행, 열을 가져야 함
    - 입력
        + Ctrl + Shift + Enter → 중괄호({}) 자동
    - 일부를 수정/이동/삭제 불가능 (전체는 가능)
    - 배열 상수 입력
        + 열 구분 → 쉼표(,)
        + 행 구분 → 세미콜론(;)
* 차트의 특징 ★
    - 차트 작성
        + 반드시 원본 데이터가 있어야 함
    - 원본 데이터가 바뀌면 차트의 모양도 바뀜
    - 계열 겹치기
        + -100% ~ 100% 사이
        + 양수: 데이터 계열 겹쳐 표시
        + 음수: 데이터 계열 사이 벌어짐
    - 간격 너비
        + 0% ~ 500%사이
        + 수치가 클수록 막대와 막대 사이의 간격이 넓어짐
        + 막대 넓이는 줄어듦
* 추세선
    - 3차원, 방사형, 원형, 도넛형, 표면형 차트에는 추가 불가
    - 종류
        + 지수, 선형, 로그, 다항식, 이동평균, 거듭제곱
    - 하나의 데이터 계열에 2개 이상의 추세선 표시 가능
* 용도별 차트 종류 ★

|차트|용도|
|:-:|:-|
|||

<details>
  <summary>워크시트 화면 10~400%까지 확대/축소 O</summary>

|틀 고정|창 나누기|
|:-:|:-:|
|데이터의 양이 많은 경우,<br>특정 열/행을 고정시켜<br>셀 포인터의 이동과 상관없이<br>화면에 항상 표시<br>인쇄 시 미적용<br>셀 포인터의 왼쪽, 위쪽으로 고정선 표시|서로 떨어져 있는 데이터를 한 화면에 표시<br>인쇄 미적용<br>창 나누기 기준선을 마우스로 더블클릭하면 창 나누기 취소|

</details>

* 여러 페이지를 한 페이지로 출력하는 방법 ★
    - 페이지 탭
        + [자동 맞춤]의 용지 너비/높이를 1로 지정
* 인쇄 영역 ★
    - 워크시트의 내용 중 특정 부분만 인쇄 가능
    - 인쇄 영역에 포함된 도형을 인쇄되지 않게
        + 도형 더블클릭 후 '도형 서식' 창의 [도형 옵션] → [크기 및 속성] → [속성]에서 '개체 인쇄' 옵션 선택 해제
* 정렬
    - 정렬 기준은 최대 64개까지 지정 가능 (기본: 행 단위)
    - 기준
        + 셀 값, 셀 색, 글꼴 색, 셀 아이콘이 있음
    - 숨겨진 행/열에 있는 데이터는 정렬에 포함되지 않음
    - 정렬 방식
        + 오름차순, 내림차순, 사용자 지정 목록
* 사용자 지정 정렬
    - 영문자 대/소문자 구분 정렬 기능
        + 오름차순에서 소문자 우선
    - 오름차순
        + 숫자 문자 논리값 오류값 빈셀 순
    - 내림차순
        + 오류값 논리값 문자 숫자 빈셀 순
    - 문자 오름차순
        + 특수문자, 영문자(소문자, 대문자), 한글 순
    - 영숫자 정렬
        + 왼쪽에서 오른쪽으로 정렬
        + (예시) 오름차순 정렬 → A1 A10 *A101* A11
* 자동 필터
    - 데이터 목록에 반드시 필드명(열 이름표) 필요
    - 2개 이상의 필드(열) 조건이 설정된 경우 *AND* 조건으로 결합 (동일 필드에는 OR 조건 가능)
* 고급 필터
    - 기본 조건 지정 방법
        + 조건을 모두 같은 행에 입력하면 AND, 다른 행에 입력하면 OR 조건
    - 고급 조건 지정 방법
        + 함수나 식의 계산 값을 고급 필터의 찾을 조건으로 지정하는 방식
* 외부 데이터 가져오기
    - 엑셀에서 가져올 수 있는 외부 데이터
        + 데이터베이스 파일 SQL/Access/dBase
        + 웹 *.html
        + XML
        + 텍스트 파일 txt/prn
        + 엑셀 파일 xlsx/티느
        + 쿼리 *.dqy
        + OLAP 큐브 파일 *.oqy
        + *X 한글 파일 X*
    - 외부 데이터 가져오기를 사용해 가져온 데이터는 원본 데이터가 변경될 경우 가져온 데이터에도 반영되도록 설정할 수 있음
* 부분합
    - 부분합을 작성
        + 첫 행에는 열 이름표 필요
        + 반드시 기준 필드 기준으로 오름차순/내림차순 정렬
    - 워크시트 왼쪽에 부분합을 계산한 하위 그룹 단위로 윤곽 설정됨 (*윤곽 기호* 표시)
    - '부분합' 대화상자 → 새로운 값으로 대치
        + 이미 작성된 부분합을 지우고, 새 부분합으로 변경
* 피벗 테이블 ★
    - 많은 데이터를 요약·분석 (한눈에 쉽게 파악 O)
    - 원본 데이터가 변경되면 새로고침을 해야 피벗 테이블의 데이터 변경 가능
    - 행/열 영역에 표시된 데이터 일부는 수정 가능, 값 영역에 표시된 데이터는 수정 불가
        + 피벗 차트 작성 시 자동으로 피벗 테이블도 작성됨
    - 피벗 테이블과 피벗 차트를 함께 만든 후 작성된 피벗 테이블을 삭제하면 피벗 차트는 일반 차트로 변경
    - 새 워크 시트에 피벗 테이블 생성
        + 보고서 필터의 위치는 [A1] 셀, 행 레이블은 [A3] 셀에서 시작
* 시나리오
    - 다양한 상황과 변수에 따른 여러 가지 결과값의 변화를 가상의 상황을 통해 예측하여 분석하는 도구
    - 변경 셀: *데이터를 변경할 셀의 범위*를 지정
    - 결과 셀: 반드시 *변경 셀을 참조하는 수식*으로 입력
        + 시나리오가 작성된 원본 데이터를 변경해도 이미 작성된 시나리오 보고서에는 반영되지 않음
    - 하나의 시나리오에 최대 32개의 변경 셀 지정 가능
* 목표값 찾기
    - 원하는 결과(목표) 값은 알고 있지만 그 결과 값을 계산하기 위해 필요한 입력값을 모를 경우 사용
    - 데이터 하나만 지정 가능
    - 목표값은 사용자가 원하는 데이터를 직접 입력
        + 특정 셀 주소 선택 X
* 데이터 표
    - 특정 값의 변화에 따른 결과값의 변화 과정을 표의 형태로 표시
        + 데이터 표 결과는 일부분만 수정 불가
* 데이터 통합
    - 비슷한 형식의 여러 데이터를 하나의 표로 통합
    - 통합 데이터의 순서/위치 통일 → 위치(기준) 통합
* 매크로 기록
    - 엑셀에서 다양한 명령들을 일련의 순서대로 기록
    - 필요할 때마다 해당 키나 도구를 이용해 기록해 둔 처리 과정이 수행되도록 하는 기능
    - 매크로 기록에 사용된 명령, 함수
        + Visual Basic 모듈에 저장되므로 내용 추가/삭제/변경 가능
        + Visual Basic Editor 실행 방법: Alt + F11
* 매크로 이름 ★
    - '매크로1, 매크로2, ..' 등과 같이 자동 부여되는 이름을 지우고 사용자가 임의 지정 가능
    - 첫글자는 반드시 문자 (이후 숫자, 문자, 밑줄 등 사용 가능)
    - */?''.-※* & 공백 사용 불가
    - 매크로 이름을 'Auto_Open'으로 지정 시 해당 통합 문서를 열 때마다 매크로가 자동 실행됨
* Visual Basic Editor에서 매크로 실행 ★
    1. F5: 일반적인 실행
    2. F8: 한 단계씩 코드 실행 → 디버깅 용
    3. Shift + F8: 프로시저 단위 실행
    4. Ctrl + F8: 모듈 창의 커서 위치까지 실행
* 프로시저
    - 특정 기능을 실행할 수 있도록 나열된 명령문의 집합
    - 모듈(Module) 안에 구성

## 3과목 데이터베이스 일반

* 데이터 베이스
    - 한 조직에 있는 여러 응용 시스템들이 공용으로 소유, 유지, 이용하는 공용 데이터

|장점|단점|
|:-|:-|
|데이터 중복 최소화<br>데이터 공유<br>일관성, 무결성 유지<br>유지보수 용이|전문가 부족<br>전산화 비용 증가<br>파일 회복이 어려움<br>복잡화, 속도가 느림|

* DBMS (DataBase Management System)
    - 파일 시스템의 단점인 데이터의 중복성, 종속성의 문제를 해결하기 위한 시스템 (백업, 회복 절차 복잡)
* 데이터베이스 언어 ★

|종류|특징|
|:-:|:-|
|데이터 정의어<br>DDL|● 데이터베이스를 생성, 수정할 때 사용<br>● 데이터베이스 관리자/설계자<br>● 논리적, 물리적 구조 정의|
|데이터 조작어<br>DML|● 데이터베이스에 저장된 데이터를 실질적으로 처리할 때 사용<br>● 데이터 처리: 검색, 삽입, 삭제, 변경 등|
|데이터 제어어<br>DCL|● 데이터 보안, 무결성, 데이터 회복, 병행 수행 제어 등 정의에 사용되는 언어<br>● 데이터베이스 관리자가 데이터 관리를 목적으로 사용|

* 관계형 데이터베이스
    - 단순한 표 이용, 데이터의 상호관계를 정의 (DB 구조)
    - 구성 형태
        + 테이블: 튜플(레코드)의 집합(릴레이션)
        + 튜플: 테이블의 행을 구성하는 개체 (레코드)
        + 속성(Attribute): 테이블 열을 구성하는 항목 (필드)
        + *도메인*: 하나의 속성에서 취할 수 있는 값의 범위
    - 차수 = 속성의 개수
    - 기수 = 튜플의 개수
* 릴레이션의 특징
    - 테이블을 구성하는 속성(필드) 간의 순서는 중요하지 않음
    - 속성명: 유일
    - 속성값: 유일하지 않음
* 키의 종류 ★
    - 후보키
        + 기본키로 사용할 수 있는 속성들
        + 릴렝션에 있는 모든 튜플에 대해 유일성과 최소성을 만족해야함
    - 기본키
        + 후보키 중 선택한 주요키
        + 기본키로 정의된 필드(속성)에는 동일한 값이 중복되어 저장할 수 없음
        + Null 값 불가
    - 외래키 (외부키)
        + 관계를 맺고 있는 테이블 R1, R2에서 테이블 R1이 참조하고 있는 테이블 R2의 기본키와 같은 R1 테이블의 속성
        + Null 값, 중복값 입력 가능
    - 대체키
        + 후보키 중 기본키를 제외한 나머지 속성
    - 슈퍼키
        + 한 테이블 내 속성들의 집합으로 구성
        + 테이블을 구성하는 모든 튜플에 대해 유일성은 만족, 최소성은 만족X

<details>
  <summary>제약 조건</summary>

- 개체 무결성
    - 기본키는 NULL 값을 가질 수 없음
- 참조 무결성
    - 외래키 값 = 참조 테이블의 기본키 값
</details>

* 정규화
    - 이상 현상이 발생하지 않게 (중복/종속성 배제 원칙)
    - 데이터베이스의 개념적/논리적 설계 단계 (물리적 설계 아님 주의)
    - 정규형에는 제1 정규형에서부터 제5 정규형까지, 단계가 높아질수록 만족시킬 제약조건 ↑ (높은 수준)
    - 정규화를 수행하여 데이터의 중복을 완전히 제거할 수는 없음
* 테이블 만들기
    - *.![]*를 제외한 특수 기호, 공백, 숫자, 문자를 조합한 모든 기호 사용 가능
    - 공백은 이름의 첫 문자로 사용 불가
    - 테이블 이름, 필드 이름 중복 가능
    - 동일 테이블 내에서 필드 이름 중복 불가!
* 사용자 지정 기호 ★
    - 
* 기본키
    - OLE 개체
    - 첨부 파일
    - 계산 형식의 필드에는 설정 불가
    - 기본키로 지정 시 자동으로 인덱스 설정
    - 관계가 설정된 테이블: 기본키를 해제할 수 없으므로 기본키를 해제하려면 먼저 설정된 관계를 제거
* 색인 (Index)
    - 데이터의 검색이나 그룹화 등의 작업 속도 향상을 위해 데이터를 일정한 기준에 맞게 정렬되도록 설정
    - 인덱스는 기본적으로 오름차순 정렬
        + 중복값이 적은 필드를 지정 → 검색 속도 향상
    - 하나의 테이블에 32개까지 인덱스 지정 가능
    - 하나의 인덱스에는 10개의 필드 사용 가능
    - 속성: 아니오, 예(중복 불가능), 예(중복 가능)
    - OLE 개체, 첨부파일, 계산 형식의 필드에는 설정 불가
        + 데이터 검색, 정렬 등의 작업 시간은 빨라지지만 데이터 추가나 변경 시 속도가 느려짐
* 참조 무결성 ★
    - 외래키 필드 값을 기본 테이블의 기본키 필드 값과 동일하게 유지해 주는 제약 조건
    - 설정 조건
        + 기본 테이블에서 사용할 필드는 기본키이거나 고유 인덱스가 설정돼 있어야 함
        + 관계 설정에 사용되는 두 테이블의 필드는 데이터 형식이 같아야 함
* 외부 데이터 가져오기
    - 데이터를 가져와도 원본 데이터는 변경 x
    - 가져온 데이터를 변경해도 원본 데이터에 영향 X
    - Excel, 텍스트 파일, HTML 문서 등은 가져오기 시 제외할 필드를 지정할 수 있음
* 외부 데이터 연결하기
    - 연결된 테이블의 데이터를 변경하면 원본 데이터도 자동으로 변경
    - 원본 데이터베이스의 데이터(레코드)를 삭제하면 연결된 테이블의 데이터도 삭제됨
* 단순 조회 질의: 기본 구문 *S-F-W* ★
    - 필드 이름
        + 테이블의 모든 필드를 검색할 경우 필드 이름 대신 '*'를 입력
        + 특정 필드들만 검색할 경우 필드와 필드를 쉼표(,)로 구분
```SQL
SELECT [DISTINCT] 필드이름
FROM 테이블 이름
[WHERE 조건식]
```
* 정렬 *S-F-W-O* ★
    - 정렬 방식
        + 오름차순 *ASC*
        + 내림차순 *DESC*
    - 정렬 방식을 지정하지 않으면 기본적으로 오름차순 정렬
    - 오름차순
        + 숫자 - 한글 - 영문(소-대문자) 순 정렬
```SQL
SELECT [DISTINCT] 필드이름
FROM 테이블 이름
[WHERE 조건식]
[ORDER BY 필드이름 정렬방식 ···]
```
* 그룹 지정 *GROUP BY & HAVING* ★
    - 일반적으로 GROUP BY는 SUM, AVG, COUNT 등 그룹 함수와 함께 사용
```SQL
SELECT [DISTINCT] 필드이름
FROM 테이블 이름
[WHERE 조건식]
[GROUP BY 필드이름]
[HAVING 그룹 조건식]
```
* 주요 함수
    - AVG (필드이름): 필드 평균
    - SUM (필드이름): 필드 합계
    - LEN (필드이름): 필드 문자열 길이 반환
    - LEFT (문자열, 자릿수): 왼쪽부터 주어진 자릿수만큼 추출
    - MID (문자열, 시작값, 자릿수): 시작값부터 주어진 자릿수만큼 추출
    - RIGHT (문자열, 자릿수): 오른쪽부터 주어진 자릿수만큼 추출
    - INSTR (문자열, 찾는 문자): 문자열에서 찾는 문자/문자열의 위치를 구함
    - VAL (문자열): 문자열로 표시된 숫자를 숫자 값으로 반환
* 특수 연산자를 이용한 질의
    - *LIKE*
        + 대표 문자를 이용해 필드값이 패턴과 일치하는 레코드만 검색
    - 대표 문자
        + \* 또는 %: 모든 문자를 대표
        + ? 또는 _: 한자리 문자를 대표
        + #: 한자리 숫자를 대표
* 조인 ★
    - 조인에 사용되는 기준 필드의 데이터 형식은 동일하거나 호환되어야 함
    - 여러 테이블을 조인할 경우 접근 속도 향상을 위해 필드 이름 앞 테이블 이름을 마침표(.)로 구분
    - *내부* 조인
        + 관계가 설정된 두 테이블에서 조인된 필드가 일치하는 행만 질의에 포함
    - *왼쪽 외부* 조인
        + 왼쪽 테이블의 모든 레코드 포함
        + 오른쪽 테이블은 조인된 필드가 일치하는 레코드만
    - *오른쪽 외부* 조인
        + 오른쪽 테이블의 모든 레코드 포함
        + 왼쪽 테이블의 일치(조인된) 레코드만 질의에 표함
* 실행 질의 ★
<details>
  <summary>삽입(INSERT)문</summary>
```SQL
INSERT INTO 테이블 이름 (필드명1, 필드명2, ...)
VALUES (필드값1, 필드값2, ...)
```
</details>
<details>
  <summary>수정(UPDATE)문</summary>
```SQL
UPDATE 테이블 이름
SET 필드명1=값1, 필드명2=값2, ...
WHERE 조건식
```
</details>
<details>
  <summary>삭제(DELETE)문</summary>
```SQL
DELETE *
FROM 테이블 이름
WHERE 조건식
```
</details>
* 크로스탭 쿼리
    - 요약 데이터를 보다 쉽게 이해할 수 있도록 합계, 평균 등 집계 함수를 계산한 다음 데이터 시트 측면과 위쪾에 두 세트의 값으로 그룹화하는 유형
* 폼 ★
    - 테이블이나 질의(쿼리), SQL문을 원본으로 하여 데이터의 입력, 수정, 삭제, 조회 등의 작업을 편리하게 수행할 수 있도록 환경을 제공하는 개체
    - 폼에서 데이터를 입력/수정하면 연결된 원본 테이블/쿼리에 반영됨
        + 폼을 통해 데이터베이스의 보안성 ↑
    - 폼은 테이블, 쿼리와 달리 이벤트 설정 가능
    - 테이블/쿼리와의 연결 여부에 따른 분류
        + 바운드(Bound) 폼: 테이블/쿼리의 레코드와 연결된 폼
        + 언바운드(Unbound) 폼: 테이블/쿼리의 레코드와 연결되지 않은 폼
* 폼 분할
    - 하나의 원본 데이터를 이용
    - 상단 열 형식
    - 하단 데이터시트 형식
    - 두개의 폼이 한 화면에 작성됨
* 하위 폼
    - 폼 안에 있는 또 하나의 폼
    - 기본이 되는 폼은 상위(기본) 폼
    - 상위(기본) 폼 안에 있는 폼이 하위 폼
    - 일대다 관계에 있는 테이블이나 쿼리를 효과적으로 표시 가능
        + '일'은 기본 폼
        + '다'는 하위 폼
    - 일반적으로 사용할 수 있는 하위 폼의 개수 제한 x
    - 하위 폼을 7개 수준까지 중첩 가능
    - ● 하위 폼에서 새로운 레코드를 추가할 때 설정해야 할 폼 속성
        + '추가 가능'을 예(Y)로 설정
* 컨트롤의 주요 속성: 탭 순서 ★
    - 폼 컨트롤에 적용할 수 있는 것, Tab이나 Enter를 눌렀을 경우 이동되는 컨트롤의 순서를 정의
    - 기본적으로 컨트롤을 작성한 순서대로 탭 순서 설정
    - '자동 순서' 단추
        + 위쪽에서 아래쪽으로, 왼쪽에서 오른쪽으로 탭 순서 자동 설정
* 조건부 서식
    - 폼이나 보고서에서 조건에 맞는 특정 컨트롤값에만 서식 적용
        + 규칙은 50개까지 지정 가능
    - 지정한 규칙 중 두개 이상이 참이면, 첫번째 규칙에 대한 서식이 적용됨
* 도메인 함수
    - → 도메인 계산 함수 (인수, 도메인, 조건식)
    - 테이블이나 쿼리, SQL 식에 의해 정의된 레코드 집합을 이용해 통계 계산을 구할 때 사용
        + DAVG 평균
        + DSUM 합계
        + DCOUNT 개수
        + DMIN 최솟값
        + DMAX 최댓값
        + DLOOKUP 특정 필드 값
* 보고서 ★
    - 이미 만들어진 테이블이나 질의 등의 데이터를 요약/그룹화하여 종이에 출력하기 위한 개체
    - 폼과 동일하게 여러 유형의 컨트롤로 데이터 표시
        + 이벤트 프로시저 작성은 가능
        + 데이터 입력, 추가, 삭제 등의 작업은 불가능
    - 보고서의 레코드 원본
        + 테이블
        + 쿼리
        + SQL 문
* 보고서의 구성 ★
    - 기본적으로 보고서 머리글, 보고서 바닥글, 본문, 페이지 머리글, 페이지 바닥글 구역과 컨트롤, 각 구역의 선택기 등
        + 보고서 머리글: 보고서 첫 페이지 상단에 *한 번만* 표시
        + 보고서 바닥글: 맨 마지막 페이지, 일반적으로 그룹별 요약 정보 표시에 사용
        + 페이지 머리글: 보고서 *모든 페이지*의 상단에 표시
* 보고서 보기
    - 출력될 보고서 미리 보기 기능
    - 화면 출력용 (종이 출력 X)
    - 인쇄 미리보기와 비슷하지만 페이지 구분없이 보고서를 모두 표시!
* 머리글/바닥글에 요약 정보 표시 ★
    - 페이지 번호 → [Page]: 현재 페이지
    - [Pages]: 전체 페이지
    - &: 식이나 문자열 연결
    - 큰따옴표(""): 큰따옴표 안의 내용을 그대로 표시
* 매크로
    - 테이블, 쿼리, 폼, 보고서 등 액세스 각 개체들을 효율적으로 자동화할 수 있도록 미리 정의된 기능을 사용하는 것
    - 매크로 함수
        + 주로 컨트롤 이벤트에 연결해 사용
    - 그룹 매크로 내의 특정 매크로를 사용
        + 그룹 매크로 이름과 매크로 이름을 마침표(.)로 구분
    - 액세스는 매크로 기록 기능 미지원 (↔ 엑셀은 지원)
* 실행 관련 및 기타 매크로 함수
    - QuitAccess: 액세스 종료
    - RunMacro: 매크로 실행
    - OpenQuery: 데이터 시트 보기, 디자인 보기, 인쇄 미리보기 등 쿼리 열기
    - OpenForm: 폼 보기, 디자인 보기, 인쇄 미리보기, 데이터시트 보기 등 폼 열기
    - MessageBox: 메시지 상자를 통해 경고나 알림 등의 정보를 표시
* 데이터 접근 개체: Recordset 개체
    - 기본 테이블이나 실행된 명령 결과로부터 얻어진 데이터를 임시로 저장해 두는 레코드 집합
    - 레코드(행) & 필드(열)을 사용해 구성

<details>
    <summary>주요 메서드</summary>

|메서드|동작|
|:-:|:-|
|Open|연결된 레코드셋 열기|
|Close|열려 있는 개체와 관련된 종속 개체를 모두 닫음|
|Update|Recordset 개체의 변경 사항 저장|
|AddNew|업데이트 가능한 Recordset 개체를 위한 새 레코드 만들기|
|Delete|현재 레코드/레코드 그룹 삭제|
|Find|Recordset에서 정의된 기준에 맞는 레코드를 차례대로 검색|
|Seek|Recordset의 인덱스를 검색하여 지정된 값에 일치하는 행을 찾고, 현재 레코드 위치를 해당 레코드로 변경|
</details>