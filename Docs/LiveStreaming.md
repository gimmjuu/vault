---
tags:
  - COMPUTERVISION
---

# LiveStreaming

## 🎇 LiveStreaming Protocol
### 📌 RTSP : Real-Time Streaming Protocol

실시간 스트리밍 프로토콜

IETF (Internet Engineering Task Force) 가 개발한 통신 규약

실시간 속성을 가진 스트리밍 데이터 전송을 제어하기 위한 방법을 제공(응용 프로그램 수준의 프로토콜)

### 📌 RTP (Real-time Transport Protocol)

RTSP를 통해 제어되는 미디어 데이터를 전송하는 데 사용되는 전송 프로토콜

오디오, 비디오 등 멀티미디어 데이터를 포함하는 미디어 서버를 원격 조작하기 위한 프로토콜

### 📌 RTSP (Real-time Transport Streaming Protocol)

오디오 및 비디오와 같은 실시간 데이터의 제어된 주문형 전송을 가능하게 하는 확장 가능한 프레임워크 제공

이때, 데이터 소스에는 실시간 데이터 피드 및 저장된 클립이 모두 포함

여러 데이터 전송 세션을 제어하고, UDP, 멀티캐스트 UDP, TCP와 같은 전송 채널을 선택할 수 있는 수단을 제공하며, RTP에 기반한 전송 메커니즘을 선택하기 위한 수단을 제공

### 📌 HLS (HTTP Live Streaming)

라이브 비디오 스트리밍 프로토콜

전용 미디어 서버 없이 일반 웹서버로도 라이브 스트리밍이 가능(★)

하나의 영상을 10초 단위로 쪼개 재생 목록을 구성한 후, 짧은 비디오 조각을 다운로드해서 재생하는 방식

#### ⚡ VMS
- VMS for IP Camera
- Model-name + VMS
- General VMS
- ODM (ONVIF Device Manager) : ONVIF protocol

#### ⚡ VLC Media Player
RTSP 서버 구축 및 라이브 스트리밍 툴

RTSP streaming을 위해 IP 주소 + ID:PWD 가 필요함

`rtsp://{ID}:{PWD}@{IP address}:{port}`

```
$ python detect.py --weights yolov8s.pt --source ‘rtsp://admin:admin@192.168.0.129:554/stream1’
```
