---
tags:
  - CS
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
