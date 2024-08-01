---
created:
    2024-07-24
tags:
    - DOCKER
---

<link rel="stylesheet" href="../Assets/style.css" />

# Dockerfile

## Sample

```dockerfile
FROM ubuntu:20.04
ENV PIP_ROOT_USER_ACTION=ignore
# Prevent interaction interruption when installing libglib2.0-0
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get install -y net-tools
RUN apt-get install -y iputils-ping
RUN apt-get install -y vim
RUN apt-get install -y python3-pip
RUN apt-get install -y libgl1-mesa-glx
RUN apt-get install -y libglib2.0-dev
RUN pip install --upgrade pip
# RUN pip install opencv-python-headless quart quart_cors
RUN pip install flask opencv-python-headless websockets ultralytics
RUN ln -snf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
# Directory where commands from RUN, CMD, and ENTRYPOINT inside the image were executed
WORKDIR /app
# Add files from the current directory to a location inside the image
ADD . /home/imr-ai2/hdd11/jhi/solar_panel
# Port Exposure
EXPOSE 6060
EXPOSE 6061
ENTRYPOINT ["python3", "run.py"]
```
