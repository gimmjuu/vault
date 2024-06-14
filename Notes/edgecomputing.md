---
tags:
    - CS
---
# 엣지 컴퓨팅 <span style="color: pink;font-style: italic;">Edge Computing</span>
- ➰　엔터 프라이즈 애플리케이션을 데이터 소스(IoT 디바이스 또는 로컬 엣지 서버)와 가까운 위치로 이동시키는 분산 컴퓨팅 프레임워크
- ➰　데이터 소스와 가까운 곳에 위치하면 인사이트 제공 속도 향상, 응답 시간 단축, 대역폭 가용성 개선 등 주요 비즈니스적 이점이 있음

## 엣지 컴퓨팅 <span style="color: pink;font-style: italic;">Edge Computing</span> 등장 배경
- 클라우드 컴퓨팅의 경우 클라우드에 자리잡은 고성능 중앙 집중 서버가 모든 데이터를 연산 처리함
- 단말기<span style="color:rgba(255,255,155,0.8);font-weight:500;font-style:italic;"> Edge</span>는 데이터를 수집해 네트워크를 통해 클라우드로 주고 받는 기능을 수행
    - PC, 스마트폰, 공장 제조라인의 센서 등
- 클라우드 컴퓨팅의 경우, 엣지의 컴퓨팅 능력은 중요하지 않음
    - 엣지는 단순히 네트워크에 접속되어 있는 단말기 역할
- <span style="color:pink;">But</span> 사물인터넷<span style="color:rgba(255,255,155,0.8);font-weight:500;font-style:italic;"> IoT </span> 기기의 확산으로 데이터 양이 폭증하면서 클라우드 컴퓨팅의 한계가 발생
    - 대용량 데이터를 실시간에 가깝게 컴퓨팅 처리하여 결과를 즉각적으로 활용하는 경우 클라우드 컴퓨팅은 부적합
    - 대신 소형 서버를 엣지 근처에 위치시켜 사용하는 것이 효율적
- 방대한 데이터를 분산된 소형 서버에서 실시간으로 처리하는 기술 등장
    - 네트워크 가장자리<span style="color:rgba(255,255,155,0.8);font-weight:500;font-style:italic;"> Edge</span>에서 데이터를 처리함
    - 자율주행차, 스마트팩토리, 가상현실(AR) 등

<center>
<img src="../Assets/edge-computing.png" alt="클라우드와 엣지 컴퓨팅 비교" height=300 style="margin:10 0"/>
</center>

- 클라우드 컴퓨팅과 엣지 컴퓨팅은 상호 보완적임
    - 클라우드 컴퓨팅은 빅데이터, 고성능 연산처리, 인터넷 검색, 메일 서비스 등 대용량 연산처리를 필요로 하나 Latency를 일정 허용하는 서비스에 적합함
    - 엣지 컴퓨팅은 증강현실, 자율주행 자동차, 실시간 의료 서비스 등 실시간 서비스 요구사항이 강한 경우 적합

# Reference
<span style="color:pink;font-weight:600;"></span>
<span style="color:rgba(255,255,155,0.8);font-weight:500;font-style:italic;"> </span>
