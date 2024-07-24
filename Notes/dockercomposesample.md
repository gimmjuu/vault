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
version: "3.4"

services:
  server:
    image: solarserver/socket:v1.0
    container_name: solar_server
    restart: always
    volumes:
      - .:/app
    ports:
      - "6060:6060"
      - "6061:6061"
    environment:
      - APP_PORT=6060
      - WS_PORT=6061
      - THERMAL_STREAM_PATH=rtsp://admin:gw[bg]imr22@115.143.44.94:54554/profile2/media.smp
      - THERMAL_MODEL_PATH=./models/solar_panel.pt
      - CCTV_STREAM_PATH=rtsp://admin:gw[bg]imr22@124.58.148.62:54554/profile4/media.smp
      - CCTV_MODEL_PATH=./models/yolov8s.pt
    privileged: true
    command: sh -c "/app/run.sh"
```