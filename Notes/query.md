---
tags:
  - DB
---
# Query
## 🎇 SQL
| |tag||query| |
|:---:|:---|:---:|:---|:---:|
| |중간값 검색| |WHERE col BETWEEN a AND b| |
| |값 수정| |UPDATE table SET col = val WHERE ...| |
| |결과 병합| |SELECT CONCAT(a, b, ...) FROM table| |
| |조건 조인| |SELECT * FROM tableA JOIN tableB ON tableA.a = tableB.b| |
| |포함 조건| |WHERE a in (values)| |
| |중복 제거| |SELECT DISTINCT a FROM tableA| |
| |그룹 정렬| |SELECT a FROM tableA GROUP BY a HAVING COUNT(a)=1| |
| |다중 정렬| |ORDER BY| |
| || |UNION ALL| |
