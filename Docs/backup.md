---
tags:
  - CS
---
Hugging Face 🤗
Python package : Transformers
$ pip install transformers
Pipeline
pipeline 함수는 허깅페이스에 업로드된 모델을 불러오는 API 기능을 제공함
from transformers import pipeline
Pre-trained 모델 사용하기
허깅페이스 페이지에서 해당 모델의 Task와 주소를 pipeline 함수에 입력하면 해당 모델을 객체로 호출함
client = pipeline("{Task}", "{model-address}")
result = client(target-data)
모델 로컬에 다운받기
허깅페이스 페이지에서 해당 모델의 File and Versions 탭으로 이동
업로드된 파일을 전체 다운로드 후,
pipline 함수 호출 시 다운 받은 로컬 경로를 지정해줌
client = pipeline(“{Task}”, model=’C:/path/to/models/target-model’)

---

##### The Memory Hierarchy
System programming
메모리 시스템 Memory System
저장 장치들의 계층구조

상위 계층일수록 작고, 비싸고, 빠른 접근이 가능함
좋은 지역성(Locality)를 갖는 프로그램은 나쁜 지역성을 가진 프로그램보다 좀 더 상위 메모리 계층에서, 더 많은 데이터에 접근하려는 경향이 있으며, 더 빨리 돌게 됨
Locality에 따라 응용 성능이 크게 달라짐

휘발성 메모리 Volatile Memory
RAM (Random Access Memory)
Volatile(휘발성) : “Lose information if the supply voltage is turned off.”
전원이 공급되지 않으면 메모리에 들어있던 정보들이 소멸
모든 데이터를 RAM에 저장하는 것은 비용과 전력 사용 면에서 비효율적
RAM의 종류
SRAM (Static RAM 정적램)
집적도가 낮고 전력 소모가 많고 구조가 복잡해서 용량이 작지만 속도는 매우 빠름
CPU의 캐시 메모리로 주로 사용
Persistent : 전원 공급만 있으면 메모리가 절대로 사라지지 않음
자외선, 전파, 전기에 강함
Lower Density : 1bit 당 트랜지스터 6개 필요
DRAM (Dynamic RAM 동적램)
집적도가 높고 전력소모가 적고 구조가 간단해서 용량이 큰 편
메인 메모리와 그래픽 시스템의 프레임 버퍼로 사용 (일반적인 PC용 메모리)
Need Refresh : 전원이 공급되어 있어도 메모리가 주기적으로 사라짐
1bit 당 트랜지스터 1개 필요 (SRAM에 비해 용량이 6배 정도 큼)
DRAM 칩 내 셀은 w DRAM 셀로 이루어진 d 슈퍼셀로 구성 ->
d × w DRAM칩은 d 슈퍼셀, 슈퍼셀당 8비트를 가짐

데이터는 ‘버스 Bus’라고 하는 공유 전기회로를 통해 CPU와 DRAM 메인 메모리 앞뒤로 교환됨
Read Transaction : 데이터를 메인메모리에서 CPU로 이동
Write Transaction : 데이터를 CPU에서 메인메모리로 이동

비휘발성 메모리 Non-volatile Memory
ROM (Read-Only Memory)
Read-Only 지만, 일부 쓰기도 가능
ROM의 종류
MROM (Mask ROM)
제품 생산 시 메모리에 내용을 기록하는 형태
최초 프로그램 이후 삭제나 재 기록이 불가능
PROM (Programmable ROM) or FPROM (Field Programmable ROM)
생산 시 메모리가 비어 있음
이후 사용자에 의해 딱 한번 프로그램 될 수 있는 메모리, 그 다음 Read-Only
EPROM (Erasable PROM) or UV EPROM (Ultraviolet EPROM)
하드웨어적으로 메모리 수명이 다할 때까지 재기록 가능
기본적으로 기록한 데이터는 사라지지 않음
장치에서 메모리를 제거하여 Ultraviolet light를 이용해 지우고 롬라이터 등의 장치를 이용해 재프로그램 가능
기록된 내용 전체를 한번에 지움
EEPROM (Electrically Erasable PROM)
전기 신호로 데이터를 지우는 PROM
바이트 단위로 내용을 지우고 기록할 수 있음
별도의 장치 없이 장착 상태 그대로 수정이 가능하지만 기록 속도가 느림
Flash Memories
EEPROM에 기반한 비휘발성 메모리
(모바일 등 전자기기에서) 빠르고 안정적인 비휘발성 저장장치의 역할을 수행
Block 단위(보통 512 byte, like Sector)로 기록하기 때문에 속도가 빠름
특징
No in-place writes; 이미 데이터가 있는 곳에 덮어쓰지 못 함
Asymmetric read-write speed; write보다 read가 훨씬 빠름
Write는 데이터 옮겨쓰기, 지우기 등을 포함
10만번 정도 지우면 사용할 수 없음 (버리는 영역)
NAND flash / NOR flash; 보통 NAND FLASH를 더 많이 씀
SSD (Solid State Dish); 하드디스크보다 비싸고 가볍고 충격에 강함
Solid State? 하드디스크와 달리 디스크들을 움직이는 부분이 없음
　하드디스크 HDD　
Disk = Platters 원판 + Spindle 회전축
Platters 원판 윗면, 아랫면 2개의 Surfaces로 데이터를 저장
각 Surface는 track으로 불리는 Concentric ring으로 구성 (10,000개 내외)
SRAM보다 40,000배, DRAM보다 2,500배 느림
디스크 동작

　SSDs : Solid State Disks　
(구성) = 한 개 이상의 플래시 메모리 칩 + 디스크 컨트롤러 FTL (Flash Translation Layer)
Page 단위로 데이터를 읽고 씀, 지우기는 Block 단위로만 가능

하나의 Page를 지우려면, 같은 Block의 나머지 페이지를 빈 Block으로 옮겨야 함
HDD와 달리 Mechanical Move가 없어 빠르고, 전력 소모도 적고, 견고함
한 Block당 10만번 이상 쓰기를 반복하면 Wear Out된다는 점과 비싸다는 점이 단점
NVRAM (Non Volatile RAM)
비휘발성 RAM : 전원을 차단해도 데이터를 유지하는 RAM
정확히는 비휘발성 SRAM(Static RAM)
집적도가 낮고 전력소모가 많고 구조가 복잡해 상대적으로 용량이 작지만 매우 빠름
하드디스크 같은 대용량 저장장치로는 부적합
메인보드에 주로 사용, 기본적인 부팅정보를 저장하는 용도
작동 방식
별도의 외부 배터리를 통해 전원 차단 시에도 데이터를 유지
EEPROM 연동을 통해 전원 차단 시 해당 내용을 EEPROM에 저장했다가 전원 공급 시 해당 내용을 읽어오는 방식

---

##### Edge Computing
### Edgd Computing
- Iot 디바이스가 네트워크엣지에서 데이터를 신속하게 처리하고 작동할 수 있도록 하는 분산형 컴퓨팅 프레임워크
### Edge Computing 특징
- 원격 위치에 있는 장치가 장치 또는 로컬 서버를 통해 네트워크 “edge”에서 데이터를 처리
- 중앙 데이터 센터에 가장 중요한 데이터만 전송하여 처리하므로 대기 시간 최소화

---

##### InfluxDB
- Basic Flux Query
모든 Flux 쿼리는 다음의 단계가 필요하다
1. data source
2. time range
3. data filters
1. data source 정의
form()
InfluxDB data source를 정의하는 함수
bucket 파라미터 필요
2. time 범위 정의
range(start, stop)
시간 연속적인 데이터를 쿼리할 때, 시간 범위를 반드시 지정해야 함
"Unbounded" 쿼리는 resource-intensive 하기 때문에, 이에 대한 방안으로  시간 범위를 지정하지 않으면, 데이터베이스에서 쿼리를 실행할 수 없다.
pipe-forward operator " |> " 를 이용하여, data source로부터 range()로 data를 연결한다. range 는 start, stop 파라미터를 가지며, negative duration 또는, timestamps를 이용한 absolute 값으로 표현한다.
3. data 필터링
filter([Predicate Function])
filter는  모든 record를 돌면서, Predicate Function 에 record를 입력하여 평가를 진행한다. 결과 값이  "true" 이면 output data로, "false"이면 걸러낸다.
※ Predicate Function : Predicate expression을 이용하여 record를 평가할 함수를 작성한다.
4. return
yield()
yield 함수는 각 script의 마지막에 위치하며, Flux가 자동적으로 data를 출력하고 가시화하도록 한다.

### Write data
## Line protocol

timestamp = 2024-01-01T09:00:00Z
Flux query language
from(bucket: “db”)
	|> range(start: -15m, stop: now())
	|> filter(fn: ( r ) => r[“_measurement”] == “sensors”) : select * from measurement
// |> filter(fn: ( r ) => r[“_field”] == “value”)
	|> filter(fn: ( r ) => r[“device”] == “weatherstation”)
	// |> aggregateWindow(every: 1m, fn: max) : @ InfluxDB UI
|> aggregateWindow(every: 1m, fn: mean)
	|> yield(name: “mean”) : result
=> SELECT mean(value) FROM sensors WHERE device = “weatherstation” AND time () GROUP BY sensor
InfluxDB: Flux Basics
from(bucket: “telemetry”)
	|> range(start: -15m)
	|> filter(fn: ® => r._measurement == “rpm”)
	|> aggregateWindow(every: 1m, fn: max)
	|> to(bucket: “telemetry-downsample”)


### InfluxDB: Downsampling
option task = {
            name: “Sunlight hourly average”,
            every: 1h,
            offset: 0m,
}
// Data source
from(bucket: “origin-db”)
	|> range(start: -task.every)
	|> filter(fn: ( r ) => r[“_measurement”] == “sensors”)
            |> filter(fn: ( r ) => r[“device”] == “miflora”)
            |> filter(fn: ( r ) => r[“sensor”] == “sunlight”)
	|> filter(fn: ( r ) => r[“_field”] == “value”)
	// Data transformation
	|> aggregateWindow(
                     every: task.every,
                     fn: mean,
                     createEmpty: false,
             )
	|> to(
                     bucket: “aggregates”,
                     fieldFn: ( r ) => ({“mean”: r._value}),
	)
	|> yield(name: “mean”)

### InfluxDB RP and CQ
alter retention policy "autogen" on "graphite" duration 24h shard duration 1h default
CREATE RETENTION POLICY "twoday" ON "graphite" DURATION 2d REPLICATION 1
CREATE RETENTION POLICY "week" ON "graphite" DURATION 7d REPLICATION 1
CREATE RETENTION POLICY "month" ON "graphite" DURATION 31d REPLICATION 1
CREATE RETENTION POLICY "year" ON "graphite" DURATION 366d REPLICATION 1
CREATE RETENTION POLICY "inf" ON "graphite" DURATION INF REPLICATION 1
CREATE CONTINUOUS QUERY "cq_twoday" ON "graphite" BEGIN SELECT mean(value) as value INTO "graphite"."twoday".:MEASUREMENT FROM /.*/ GROUP BY time(60s),* END
CREATE CONTINUOUS QUERY "cq_week" ON "graphite" BEGIN SELECT mean(value) as value INTO "graphite"."week".:MEASUREMENT FROM /.*/ GROUP BY time(300s),* END
CREATE CONTINUOUS QUERY "cq_month" ON "graphite" BEGIN SELECT mean(value) as value INTO "graphite"."month".:MEASUREMENT FROM /.*/ GROUP BY time(1800s),* END
CREATE CONTINUOUS QUERY "cq_year" ON "graphite" BEGIN SELECT mean(value) as value INTO "graphite"."year".:MEASUREMENT FROM /.*/ GROUP BY time(21600s),* END
CREATE CONTINUOUS QUERY "cq_inf" ON "graphite" BEGIN SELECT mean(value) as value INTO "graphite"."inf".:MEASUREMENT FROM /.*/ GROUP BY time(43200s),* END

---

##### Python
### Generator(제너레이터, 생성자)
- 메모리를 효율적으로 사용하는 Iterator(이터레이터, 반복자)
yield : 제너레이터 생성 키워드
- 제너레이터 : 여러 개의 데이터를 미리 만들어 놓지 않고 필요할 때 하나씩 생성하는 객체 (게으른 반복자iterator)
- 결과값을 나누어 얻기 때문에 성능 측면에서 이점
- 결과값을 하나씩 메모리에 올려놓기 때문에 메모리 효율 측면에서도 이점
- yield from list() : 리스트를 바로 제너레이터로 변환
- 제너레이터 표현식 : abc = (char for char in "ABC") -> 리스트 표현식과 다르게 소괄호로 표현

### Win32com


'''Python
# Import our libraries
import win32com.client as win32
import pythoncom

# Define our Application Events
class ApplicationEvents:
    # Define an event inside of our application
    def OnSheetActivate(self, *args):
        print("Activated new sheet")

# Define our Workbook Events
class WorkbookEvents:
    # Define an event inside of our Workbook
    def OnSheetSelectionActivate(self, *args):
        # Print the arguments
        print(args)
        print(args[1].Address)
        args[0].Range("A1").Value = f"You selected cell {args[1].Address}"
        print("Activated new sheet")

# Get the active instance of Excel
xl = win32.GetActiveObject("Excel.Application")
# Assign our event to the Excel Application Object
xl_events = win32.WithEvents(xl, ApplicationEvents)

# Grab the workbook
xl_workbook = xl.Workbooks("Book1")
# Assign events to workbook
xl_workbook_events = win32.WithEvents(xl_workbook, WorkbookEvents)
# While there are messages keep displaying them
while True:
    # Display the message
    pythoncom.PumpWaitingMessages()
'''

---

##### subprocessing
- Python pkg
현재 소스코드 안에서 다른 프로세스를 실행해주며 그 과정에서 데이터의 입출력을 제어하기 위해 사용
기존 system, os pkg를 사용하던 기능을 python3부터 보안상의 이유로 subprocess로 이관
→ subprocess 모듈은 새로운 프로세스를 실행하도록 도와주고 프로세스의 입출력 및 에러 등 리턴 코드를 유저가 직접 제어하게 해주는 모듈이다.
Python3.5부터 subprocess.run() 메소드를 통해 subprocess를 실행
쉘 명령어 실행
Import subprocess

# Linux : 
subprocess.run([‘ls’], shell=True, check=True)
# Windows :
subprocess.run([‘dir’], shell=True, check=True)

파이썬 소스코드 실행
Import subprocess

subprocess.run([‘test.py’], shell=True)

subprocess.check_output() 메소드를 사용해 커맨드 실행 결과를 메모리상 저장하고 반환
실행 결과 반환
Import subprocess

Result = subprocess.check_output([‘test.py’], shell=True, encoding=’utf-8’)

# run 메소드의 인수
Import sys
Result = subprocess.run(args=[sys.executable, ‘test.py’], capture_output=True)

Popen
유연한 서브프로세스를 실행하고 결과를 반환
