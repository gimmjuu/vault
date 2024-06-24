---
created:
  2024-06-24
tags:
  - BOARD
---

<link rel="stylesheet" href="../Assets/style.css" />

# Raspberry pi 5

## Basic Setup

- Raspberry Pi 5 + Active Cooler + AI Kit 조립
- [OS Installation](https://m.blog.naver.com/PostView.naver?blogId=icbanq&logNo=223382909813&fromRecommendationType=category&targetRecommendationDetailCode=1000)
- Connection: DVC 전원 + HDMI + Monitor + Keyboard + mouse

## $cf.$ docker for rpi 5

- Install

```bash
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

- Uninstall

```bash
sudo apt remove docker-ce*      # Remove the pkg
sudo ip link delete docker0     # Remove the docker0 network interface
```

## Tutorial: hailo-rpi5-examples

- [Hailo-AI GitHub](https://github.com/hailo-ai/hailo-rpi5-examples?tab=readme-ov-file)
    1. [Setup rpi and hailo](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
    2. [Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#hailo-rpi5-basic-pipelines)

# Additional Tutorial

- [How to build a Raspberry Pi NAS](https://www.raspberrypi.com/tutorials/nas-box-raspberry-pi-tutorial/)
