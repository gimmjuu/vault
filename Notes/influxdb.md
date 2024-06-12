---
tags:
    - DB
    - 시계열
---

# 💾 InfluxDB
## 🎇 Basic Flux Query
➰　모든 Flux 쿼리는 다음의 단계가 필요하다.

    1. data source
    2. time range
    3. data filters

### 📌 `data source` 정의

    form()

- InfluxDB data source를 정의하는 함수
- `bucket` 파라미터 필요

### 📌 `time` 범위(range) 정의

    range(start, stop)

- 시간 연속적인 데이터를 쿼리할 때, 시간 범위를 반드시 지정해야 함
- "**Unbounded**" 쿼리는 resource-intensive 하기 때문에,
    - 시간 범위를 지정하지 않으면 데이터베이스에서 쿼리를 실행할 수 없다.
- pipe-forward operator "**|>**" 를 이용하여, data source로부터 range()로 data를 연결한다.
- **range** 는 *start, stop* 파라미터를 가지며,
    - negative duration 또는 timestamps를 이용한 absolute 값으로 표현한다.

### 📌 data 필터링

    filter([Predicate Function])

- "**filter**"는 모든 record를 돌면서, *Predicate Function* 에 record를 입력하여 평가를 진행한다.
- 결과 값이  "*true*" 이면 output data로, "*false*"이면 걸러낸다.
- ➰　Predicate Function
    - Predicate expression을 이용하여 record를 평가할 함수를 작성한다.

### 📌 return 결과값 반환

    yield()

- "**yield**" 함수는 각 *script의 마지막*에 위치하며,
    - Flux가 자동적으로 data를 출력하고 가시화하도록 한다.

## 🎇 Influx Terms
- ➰　Measurement
    - like `table`
- ➰　Tag
    - 메타데이터
- ➰　Field
    - `value`
- ➰　Key
    - column header
- ➰　Value
    - `attribute`
- ➰　Set
    - Key-Value pair


## 🎇 Write data
### 📌 Line protocol
```
timestamp = 2024-01-01T09:00:00Z
```
- Flux query language
```sql
from(bucket: “db”)
	|> range(start: -15m, stop: now())
	|> filter(fn: ( r ) => r[“_measurement”] == “sensors”) : select * from measurement
// |> filter(fn: ( r ) => r[“_field”] == “value”)
	|> filter(fn: ( r ) => r[“device”] == “weatherstation”)
	// |> aggregateWindow(every: 1m, fn: max) : @ InfluxDB UI
|> aggregateWindow(every: 1m, fn: mean)
	|> yield(name: “mean”) : result
=> SELECT mean(value) FROM sensors WHERE device = “weatherstation” AND time () GROUP BY sensor
```
- InfluxDB: Flux Basics
```sql
from(bucket: “telemetry”)
	|> range(start: -15m)
	|> filter(fn: ® => r._measurement == “rpm”)
	|> aggregateWindow(every: 1m, fn: max)
	|> to(bucket: “telemetry-downsample”)
```

## 🎇 InfluxDB: Downsampling
```
option task = {
            name: “Sunlight hourly average”,
            every: 1h,
            offset: 0m,
}
```
```sql
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
```

## 🎇 InfluxDB RP and CQ
```sql
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
```

## 🚩 Reference
- [용어 설명 더보기](https://m.blog.naver.com/alice_k106/221226137412)
