##### 자료구조와 알고리즘

### 자료구조

## 스택
## 큐
## 트리
## 그래프

### 알고리즘

---

##### Linux

- Focusing on Command

### 기본 개념 정리

> shell
> 명령어 처리기
> 종류 : sh, bash, zbash, ksh, csh 등


> shell script
> shell을 사용해 실행할 명령을 텍스트로 작성하여 저장한 프로그램
> sciript : interpreter 방식으로 동작하는, 컴파일 되지 않는 프로그램


> sh
> 프롬프트 $
> Bourne shell의 줄임말로 sh라고 부름


> bash
> 프롬프트 #
> Bourne-agin shell (줄여서 bash)
> sh와 대부분 호환됨
> bashrc : bash를 사용할 때, bash가 참고할 사항을 정의해 놓은 파일


> zbash
> 프롬프트 %
> bash, ksh, tcsh 등 일부 기능을 포함하고 개선한 확장형 쉘
> command prompt에서 자동완성을 지원

### 자주 사용하는 명령어

Linux에서 listen 중인 Port 확인
$ sudo ss -lntu
Buffer/Cache Memory 확인
$ free -m
Buffer/Cache Memory 삭제
$ sudo sysctl -w vm.drop_caches=3

Directory 접근 권한 부여
$ sudo chown -R {user} {directory}
(예시) sudo chown -R $USER $(pwd)

---

##### Docker

- Focusing on Docker Containers

### 기본 용어 정리

## Docker

Go 언어로 작성된 Linux Container 기반 오픈소스 가상화 플랫폼
> Docker를 사용하는 이유 : 성능 향상, 뛰어난 이식성, scale out의 유연성
> 기존 os 가상화와 달리 process를 격리하여 훨씬 빠른 가상환경 구축이 가능하다.

## Docker Container

$ docker run 으로 실행한 하나의 프로세스 & 어떤 명령어를 실행하는 환경
각 container는 독립적이다. ='상태를 공유하지 않는다'
> container의 생애주기는 “생성-실행-종료-삭제”

## Docker Image

컨테이너를 실행할 수 있는 실행 파일과 설정값들을 담은 것 (like application)
> Image를 컨테이너에 담고 실행하면 해당 프로세스가 동작한다.
> 상태값을 가지지 않으며 변하지 않는다.

## Docker File

이미지 생성 출발점, Docker Image 생성을 위한 스크립트(설정 파일)
> 이미지를 구성하기 위한 명령어들을 토대로 Docker File을 작성하여 빌드하면 Docker는 Dockerfile에 나열된 명령문을 순차적으로 수행하여 Docker Image를 구성한다.
> Docker Hub에서 배포하는 Docker Image를 보완 또는 Image를 새롭게 구성하기 위해 사용한다.
> 저장 용량이 큰 Docker Image보다 Dockerfile이 용량면에서 배포에 유리하다.

## Docker Hub
공유 저장소, image를 저장하고 관리한다. (GitHub와 유사)
> image를 pull 한다.

## Docker Registry
Docker Hub와 달리 비공개적으로 격리된 저장소

[ 참조 ]
https://www.lainyzine.com/ko/article/summary-of-how-to-use-docker/
MAC에서 Docker Desktop 설치하기 https://www.lainyzine.com/ko/article/how-to-install-docker-for-m1-apple-silicon/


### Docker 명령어 형식

$ docker { sub-command } { options }

$ docker run
Linux Container 실행 명령어
지시한 이미지를 기반으로 하위 명령어를 실행하여 결과를 출력하고 종료한다.
docker run 명령어 하나가 하나의 컨테이너이다.
bash -c
여러 명령어를 한 줄에서 실행한다.

$ docker ps
Container 목록을 출력한다.
default 설정으로 실행 중인 container만 출력한다.
-a
종료된 container까지 출력한다.

$ docker exec
Docker Container 환경 탐색 (디버깅)
-it
터미널에서 작업 수행 가능
exit
디버깅 중인 container에서 탈출


### 기본 명령어

## Docker 설치
$ curl -fsSL https://get.docker.com | sudo sh

## Docker 서비스 재시작
$ sudo systemctl restart docker.socket docker.service

## Docker Image 목록 출력
$ docker images
$ docker image is

## Docker Image 내려받기
$ docker pull {Image:tag}

## Container 목록 출력
$ docker ps (-a)
$ docker container is

## Container 실행 - run
$ docker run -it -d --gpus all --name {container-name} -p 8888:8888 -v $(pwd):/workspace {image:tag}
-it
-i : 표준 입력 활성화
-t : bash 사용
-d
d for daemon
데몬모드 백그라운드 실행
--gpus
container에서 호스트 NVIDIA GPU 사용 설정
--name
container name
-p
port, 포트포워딩
-v
volume, 호스트와 컨테이너 디렉토리 바인딩
--rm
process 종료 시점에 container 자동 삭제

## Container 시작
$ docker start {container}

## Container 실행 : exec
$ docker exec -it {container} bash
$ docker exec -it {container} jupyter notebook --ip 0.0.0.0 --allow-root

## Container Log 출력
$ docker logs -f {container}

## Container 종료
$ docker stop container-name
container-id 작성도 가능하며, 식별이 가능하다면 전체 id를 입력하지 않아도 무방하다.

## Container 강제 종료
$ docker kill container-name

## Container 삭제
$ docker rm container-name
-f
실행 중인 컨테이너 강제 종료 후 삭제
중지된 Container 일괄 삭제
$ docker container prune
$ prune system?

### Docker : GPU 사용 시 메모리 확보

## Memory 확인
$ free -m

## Cache memory 삭제
- 버퍼 캐시 삭제
$ sudo echo 2 > sudo /proc/sys/vm/drop_caches
$ sudo sysctl -w vm.drop_caches=2
- 페이지 캐시까지 삭제
$ sudo echo 3 > sudo /proc/sys/vm/drop_caches
$ sudo sudo sysctl -w vm.drop_caches=3

## GPU 사용 확인
$  nvidia-smi

## 강제 종료한 프로세스 삭제
- 실행한 Process 목록 출력
$ ps aux | grep python
- 강제 종료한 Process 삭제
$ sudo kill -9 {process id}

### Docker : jupyter-notebook

## 방법 1 : etc container + jupyter module
$ docker run -it -d - -name {jupyter} - -gqus all -p 8888:8888 -v $(pwd):/workspace {non-jupyter-image:tag}
$ docker exec -it {jupyter} bash
$ pip install jupyter
$ jupyter notebook - -ip 0.0.0.0 - -allow-root
이미 생성한 컨테이너에서,
$ docker exec -it {jupyter} jupyter notebook - -ip 0.0.0.0 - -allow-root

## 방법 2 : jupyter-notebook container
$ docker run -it -d - -name {jupyter} - -gqus all -p 8888:8888 -v $(pwd):/workspace {jupyter-image:tag}
$ docker logs -f {jupyter}

- finally
컨테이너 실행 후 jupyter-notebook url로 접속 및 token 입력

### Docker 컨테이너 사용자 계정 추가
- Docker container를 root 이외의 사용자로 사용하고 싶을 때
① root 계정의 일반 컨테이너 실행
$ docker run -it <image name> bash

② 실행한 컨테이너에 새로운 사용자 정보 등록
$ adduser <user name>
. . .
$ exit

③ 사용자 계정을 추가한 컨테이너를 도커 이미지로 빌드
$ docker commit <container id> <new image name>

④ 해당 도커 이미지로 컨테이너 생성
$ docker run -it -d --gpus all --name <container name> -u <user name> <new image name>

⑤ 컨테이너 실행
$ docker exec -it <new image name> bash
=> jhi@96a737f944ac:/$

### Dockerfile
## Dockerfile 작성
- Dockerfile 작성 시 파일 이름은 반드시 “Dockerfile” 이어야 한다.
$ mkdir {apache-dockerfile} && cd {apache-dockerfile}
$ vi Dockerfile
## Dockerfile 명령어
FROM
베이스 이미지
{base-image:tag}
MAINTAINER
Dockerfile 작성자
{작성자 <메일 주소>}
LABEL
이미지 메타데이터 추가 (Key-Value 형태)
RUN
새로운 레이어에서 명령어 실행 && 새로운 이미지 생성
install -y : 무조건 설치 가능
WORKDIR
작업 디렉토리 지정
EXPOSE
Dockerfile 이미지에서 열어줄 포트를 의미
컨테이너 생성 시 -p 옵션에 EXPOSE 값을 넣어야 함
COPY/ADD
빌드 명령 도중 호스트의 파일 또는 폴더를 이미지에 가져옴
일반적으로는 COPY 사용 권장
ADD는 압축파일 혹은 네트워크 상의 파일도 가져올 수 있음
CMD/ENTRYPOINT
컨테이너 실행 시 실행할 명령어, 보통 컨테이너 내부에서 항상 돌아가는 서버를 띄울 때 사용
CMD
Container를 생성 (docker run) 할 때만 실행
Container 생성 시 추가적인 명령어에 따라 설정한 명령어를 수정할 수 있음
$ CMD [“{command}”, “{parameter 1}”, “{parameter 2}”]
$ CMD {command} {parameter 1} {parameter 2}
ENTRYPOINT
Container를 시작 (docker start) 할 때마다 실행
컨테이너 시작 시 추가적인 명령어 여부와 관계없이 무조건 실행
$ ENTRYPOINT [“{command}”, “{parameter 1}”, “{parameter 2}”]
$ ENTRYPOINT {command} {parameter 1} {parameter 2}
Dockerfile로 Docker Image Build
$ docker build -t {image-name:tag} {Dockerfile-path}
$ docker images 명령어로 이미지 생성 확인

### Docker Compose
단일 서버에서 여러 개의 Container를 하나의 서비스로 정의해 Bundle로 관리할 수 있도록 작업 환경을 제공하는 관리 도구
Container 수와 정의해야 할  옵션이 많을 때, Docker Compose를 사용해 한번에 묶어서 작업환경을 구축한다.
Docker Compose의 장점 : 하나의 정의로 여러 곳에서 동일 동작을 보장한다.

## Docker Compose 설치
- 설치
$ sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname - s)-$(uname -m)" -o /usr/local/bin/docker-compose
- 권한 부여
$ sudo chmod +x /usr/local/bin/docker-compose
- 백업 파일 이동
$ sudo mv /usr/bin/docker-compose /usr/bin/docker-compose.backup
- 심볼릭 링크 설정
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
- 버전 확인 (최신 docker는 compose를 내장한다!)
$ docker-compose --version

## Docker Compose .yml 작성
기존 Docker run 명령어를 YAML로 변환하는 방식
-> docker compose는 탭을 인식하지 못 하기 때문에, YAML 파일 들여쓰기 시 공백 2개로 구분
- version
YAML 파일 포맷 버전, Docker Compose 지원 최신 버전으로 설정
version 1 : 버전 생략
version 2 : 마이너 버전 설정 필요(2.x), 마이너 버전 생략 시 2.0 적용
version 3 : 도커 스웜과 함께 사용하도록 디자인
- services
생성될 Container들을 묶는 단위
- command
백그라운드 종료 방지, sleep infinity로 설정
- image
Service Container를 생성할 때 사용할 이미지
- environment
docker 명령어 -e 옵션과 동일, Service Container 내부에서 사용할 환경 변수 지정
- volumes
docker 명령어 -v 옵션과 동일, 호스트와 컨테이너 디렉토리 바인딩
- depends_on
특정 컨테이너에 대한 의존 관계, 명시한 컨테이너 먼저 생성한 후 실행

## Docker Compose 명령어
Docker Compose 명령어는 원래 $ docker-compose 이지만, docker 버전 업데이트 이후 $ docker compose 가 가능해짐
# 프로젝트 실행
yml 파일 필요
$ docker compose -p {project-name} up -d
-d
Docker Compose Project를 background에서 실행
-p {project-name}
프로젝트 이름을 변경하여 실행
--force-recreate
컨테이너를 지우고 새로 생성
--build
서비스 시작 전 이미지를 새로 생성
--env-file
환경변수 파일 지정
-f
기본으로 제공하는 docker-compose.yml이 아닌 별도의 파일명 실행
$ docker compose -f docker-compose.yml -f docker-compose-test.yml up
두 개의 YAML 파일 실행
YAML 파일을 두 개 이상 설정하는 경우 뒤에 오는 설정 파일이 우선됨
프로젝트 종료
$ docker compose down -v
-v or --volume
프로젝트 종료 시 네트워크, 볼륨 제거
모든 설정을 초기화하고 새로 시작할 때 사용한다.
DB 데이터 초기화에 용이하다.
- 서비스 시작
$ docker compose start
- 서비스 멈춤
$ docker compose stop
- 프로젝트 목록 확인
현재 환경에서 실행 중인 서비스 각각의 상태를 표시한다.
$ docker compose ps
- 프로젝트 로그 확인
$ docker compose logs
-f or --follow
실시간 로그 출력
- 프로젝트 Container 명령어 실행
$ docker compose exec {service} {command}
- 프로젝트 Container 명령어 일회성 실행
$ docker compose run {service} {command}
--rm
명령어 종료 후 컨테이너 종료

> $ run
> 실행 시 새로운 컨테이너를 띄움
> batch 성 작업에 특화

> $ exec
> 실행되어 있는 컨테이너에 접속
> 프로세서를 실행시켜 놓을 때 사용


## 프로젝트 구성 확인
Docker Compose에 최종 적용된 설정 확인
보통 여러 개의 YAML 파일을 실행한 경우 최종 설정값을 확인하기 위해 사용
$ docker compose config

## Docker Compose .env
Docker Compose에도 각 실행 환경에 따라 변경되어야 하는 옵션들이 있을 수 있다.
이때, 파일을 일일이 수정하는 건 비효율적
따라서, 실행 환경마다 환경변수 파일을 정의하여 동일한 Compose 파일로 각 환경에서 동적인 옵션 실행이 가능하게 한다.
# .env file
Docker Compose Project를 실행 (up) 한 상태에서,
$ cat .env
$ cat docker-compose.yml
$ docker-compose config

---

##### DeepLearning
- Focusing on model training

[ 기본 용어 정리 ]
https://wikidocs.net/book/5942

[ Training Flow ]
1. Data preprocessing
2. Model building
3. Model compiling
    a. Initialized model (*architecture) save
    b. Set callback function
4. Model fitting
    a. Trained model (*full model or only weight) save
5. Model predicting or evaluating


### Image Classification Model
## ResNet
네트워크의 크기를 키워(파라미터를 늘려) 모델 성능을 높인다.
## VGG
상하좌우 및 중심을 볼 수 있는 가장 작은 컨볼루션 필터(3x3, 1 strides) 만으로 깊은 레이어(16-19 layers)를 만들어 성능을 높인다.
## Xception
파라미터를 효율적으로 활용하여 성능을 높이기 위해 Depthwise Separable Convolution을 활용한다.
## MobileNet
효율적인 연산을 위해 Depthwise Separable Convolution을 적절히 활용하면서, 모바일 기기에서 돌아갈 수 있을만큼 경량한 구조를 설계하는데 집중했다.

---

##### YOLO
Object Detection

### YOLO v5
- YOLO(You Only Look Once) 컴퓨터 비전 모델 제품군의 하나
- 객체 감지(Object Detection)에 사용한다.
- YOLO v3 PyTorch의 확장 (객체 주위에 상자를 그리고 해당 클래스를 예측한다.)

Yolo 신경망 주요 3 구조
    = BackBone : 세분성으로 이미지 특징을 집계하고 형성하는 컨볼루션 신경망
    = Neck : 이미지 특징을 혼합하고 결합하여 예측에 전달하는 일련의 레이어
    = Head : 목의 특징을 소비하고 상자 및 클래스 예측 단계를 수행
    (-> input > backbone > neck > dense prediction > sparse prediction)

Training Step
데이터 증강(Data Augmentation -> 스케일링, 색 공간 조정, 모자이크 확대 등)
손실 계산(손실 함수를 계산하여 평균 정밀도를 제공)

CSP BackBone
    = DenseNet 기반
    = 중복 그라디언트 문제를 해결하기 위해 비슷한 중요도에 대한 매개변수와 FLOPS를 축소한다.
    (-> 추론속도 증가, 모델 크기 감소)

PA-Net Neck

YOLO v5 라벨링
    = 형식 : TXT(coco) 및 YAML
    = Tool : Roboflow (cf. VOTT, LabelImg, CVAT)

torch.hub.load(“ultralytics/yolov5”, 'yolov5s').to('cuda')

### YOLO v8
! pip install ultralytics


Pretrained Model Classes List
model = YOLO(“yolov8.pt”)
classes = model.model.names

YOLO(“yolov8n.pt”)

## Error Note
ImportError: libGL.so.1
Docker 환경에서 OpenCV를 사용할 때, Docker에 누락된 cv2 종속성 으로 인한 문제
(해결) $ apt-get update && apt-get install libgl1

---

##### LiveStreaming
ComputerVision

### LiveStreaming Protocol
RTSP : Real-Time Streaming Protocol
실시간 스트리밍 프로토콜
IETF (Internet Engineering Task Force) 가 개발한 통신 규약
실시간 속성을 가진 스트리밍 데이터 전송을 제어하기 위한 방법을 제공(응용 프로그램 수준의 프로토콜)
RTP (Real-time Transport Protocol) : RTSP를 통해 제어되는 미디어 데이터를 전송하는 데 사용되는 전송 프로토콜
오디오, 비디오 등 멀티미디어 데이터를 포함하는 미디어 서버를 원격 조작하기 위한 프로토콜
RTSP : Real-time Transport Streaming Protocol
오디오 및 비디오와 같은 실시간 데이터의 제어된 주문형 전송을 가능하게 하는 확장 가능한 프레임워크 제공
이때, 데이터 소스에는 실시간 데이터 피드 및 저장된 클립이 모두 포함
여러 데이터 전송 세션을 제어하고, UDP, 멀티캐스트 UDP, TCP와 같은 전송 채널을 선택할 수 있는 수단을 제공하며, RTP에 기반한 전송 메커니즘을 선택하기 위한 수단을 제공
HLS : HTTP Live Streaming
라이브 비디오 스트리밍 프로토콜
전용 미디어 서버 없이 일반 웹서버로도 라이브 스트리밍이 가능(★)
하나의 영상을 10초 단위로 쪼개 재생 목록을 구성한 후, 짧은 비디오 조각을 다운로드해서 재생하는 방식

1. VMS for IP Camera
2. Model-name + VMS
3. General VMS

ODM (ONVIF Device Manager)
ONVIF protocol
VLC Media Player
RTSP 서버 구축 및 라이브 스트리밍
RTSP streaming을 위해 IP 주소 + ID:PWD 가 필요함
rtsp://{ID}:{PWD}@{IP address}:{port}
(예시)
$ python detect.py --weights yolov8s.pt --source ‘rtsp://admin:admin@192.168.0.129:554/stream1’

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
