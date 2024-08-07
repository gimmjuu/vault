---
created:
    2024-07-24
tags:
    - DOCKER
---

<link rel="stylesheet" href="../Assets/style.css" />

# Docker-compose

## Sample

```yml
version: "3.4"  # compose version

services:
  server:   # app name
    image: solarserver/socket:v1.0  # for container building
    container_name: solar_server    # container name(--name)
    restart: always
    volumes:          # 볼륨(-v)
      - .:/app
    ports:            # 포트 포워딩(-p)
      - "6060:6060"
      - "6061:6061"
    # network_mode: "host"  # 호스트 네트워크 모드
    # ports 랑 network_mode 둘 중에 하나만 써야 함
    environment:      # 환경 변수 등록(-e)
      # APP -------------------
      - APP_PORT=6060
      - WS_PORT=6061
      - THERMAL_STREAM_PATH=rtsp://admin:gw[bg]imr22@115.143.44.94:54554/profile2/media.smp
      - THERMAL_MODEL_PATH=./models/solar_panel.pt
      - CCTV_STREAM_PATH=rtsp://admin:gw[bg]imr22@124.58.148.62:54554/profile4/media.smp
      - CCTV_MODEL_PATH=./models/yolov8s.pt
      # DB --------------------
      - DB_HOST=192.168.0.170
      # - DB_HOST=localhost # <- network_mode: "host"
      - DB_DATABSE=test
      - DB_USER=postgres
      - DB_PASSWORD=0000
      - DB_PORT=5432
    privileged: true
    command: sh -c "/app/run.sh"  # container 생성 후 실행할 명령어
```
