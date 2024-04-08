# Docker
---
> Focusing on Docker Containers

## 기본 용어 정리

#### Docker

- Go 언어로 작성된 *Linux Container* 기반 오픈소스 가상화 플랫폼
	- Docker를 사용하는 이유 : 성능 향상, 뛰어난 이식성, scale out의 유연성
	- 기존 os 가상화와 달리 process를 격리하여 훨씬 빠른 가상 환경 구축이 가능하다.

#### Docker Container

- `$ docker run`으로 실행한 하나의 프로세스 && 어떤 명령어를 실행하는 환경
- 각 container는 독립적이다.  ==상태를 공유하지 않는다
	- container의 생애 주기는 ***생성→실행→종료→삭제***

#### Docker Image

- 컨테이너를 실행할 수 있는 실행 파일과 설정 값을 담은 것 *like application*
	- Image를 컨테이너에 담고 실행하면 해당 프로세스가 동작한다.
	- 상태 값을 가지지 않으며 변하지 않는다.

#### Docker File

- 이미지 생성 출발점, Docker Image 생성을 위한 스크립트(설정 파일)
	- 이미지를 구성하기 위한 명령어들을 토대로 Docker File을 작성하여 빌드 하면 Docker는 Dockerfile에 나열된 명령문을 순차적으로 수행하여 Docker Image를 구성한다.
	- Docker Hub에서 배포하는 Docker Image를 보완 또는 Image를 새롭게 구성하기 위해 사용한다.
    - 저장 용량이 큰 Docker Image보다 Dockerfile이 용량 면에서 배포에 유리하다.

#### Docker Hub

- 공유 저장소, Image를 저장하고 관리한다. (GitHub와 유사)
	- Image를 pull 한다.

#### Docker Registry

- Docker Hub와 달리 비공개로 격리된 저장소

[ 참조 ]
- [https://www.lainyzine.com/ko/article/summary-of-how-to-use-docker/](https://www.lainyzine.com/ko/article/summary-of-how-to-use-docker/)
- [MAC에서 Docker Desktop 설치하기](https://www.lainyzine.com/ko/article/how-to-install-docker-for-m1-apple-silicon/)

### Docker 명령어 형식

#### $ docker { sub-command } { options }

#### $ docker run

- Linux Container 실행 명령어
- 지시한 이미지를 기반으로 하위 명령어를 실행하여 결과를 출력하고 종료한다.  
    docker run 명령어 하나가 하나의 컨테이너이다.
- bash -c
- 여러 명령어를 한 줄에서 실행한다.

#### $ docker ps

- Container 목록을 출력한다.  
    default 설정으로 실행 중인 container만 출력한다.
- -a
- 종료된 container까지 출력한다.

#### $ docker exec

- Docker Container 환경 탐색 (디버깅)
- -it
- 터미널에서 작업 수행 가능
- exit
- 디버깅 중인 container에서 탈출

### 기본 명령어

#### Docker 설치
> `$ curl -fsSL [https://get.docker.com](https://get.docker.com) | sudo sh`

#### Docker 서비스 재 시작
> `$ sudo systemctl restart docker.socket docker.service`

#### Docker Image 목록 출력
> `$ docker images`

> `$ docker image is`

> **Docker Image 내려받기**
> `$ docker pull {Image:tag}`

#### Container 목록 출력
> `$ docker ps (-a)`

> `$ docker container is`

#### Container 실행 - run
> `$ docker run -it -d --gpus all --name {container-name} -p 8888:8888 -v $(pwd):/workspace {image:tag}`
- ***-it***
	- -i : 표준 입력 활성화
	- -t : bash 사용
- -d
	- d for daemon, 데몬 모드 백그라운드 실행
- --gpus
	- container에서 호스트 NVIDIA GPU 사용 설정
- --name
	- container name
- -p
	- port, 포트포워딩
- -v
	- volume, 호스트와 컨테이너 디렉토리 바인딩
- --rm
	- process 종료 시점에 container 자동 삭제

#### Container 시작
> `$ docker start {container}`

#### Container 실행 : exec
> `$ docker exec -it {container} bash`

> `$ docker exec -it {container} jupyter notebook --ip 0.0.0.0 --allow-root`

#### Container Log 출력
> `$ docker logs -f {container}`

#### Container 종료
> `$ docker stop container-name`
- container-name 대신 container-id 사용 가능
- container-id 사용 시, 식별 가능하다면 전체 id를 입력하지 않아도 무방

#### Container 강제 종료
> `$ docker kill container-name`

#### Container 삭제
> `$ docker rm container-name`
- -f
	- 실행 중인 컨테이너 강제 종료 후 삭제

#### 중지된 Container 일괄 삭제
> `$ docker container prune`

> `$ prune system?`

### Docker : GPU 사용 시 메모리 확보

#### Memory 확인
> `$ free -m`

#### Cache memory 삭제

##### 1. 버퍼 캐시 삭제
> `$ sudo echo 2 > sudo /proc/sys/vm/drop_caches`

> `$ sudo sysctl -w vm.drop_caches=2`

##### 2. 페이지 캐시까지 삭제
> `$ sudo echo 3 > sudo /proc/sys/vm/drop_caches`

> `$ sudo sudo sysctl -w vm.drop_caches=3`

#### GPU 사용 확인
> `$  nvidia-smi`

#### 강제 종료한 프로세스 삭제

##### 1. 실행한 Process 목록 출력
> `$ ps aux | grep python`

##### 2. 강제 종료한 Process 삭제
> `$ sudo kill -9 {process id}`

### Docker : jupyter-notebook

#### 방법 1 : etc container + jupyter module
> `$ docker run -it -d - -name {jupyter} - -gqus all -p 8888:8888 -v $(pwd):/workspace {non-jupyter-image:tag}`

> `$ docker exec -it {jupyter} bash`

> `$ pip install jupyter`

> `$ jupyter notebook - -ip 0.0.0.0 - -allow-root`

> ***[other case] 이미 생성한 컨테이너***
> `$ docker exec -it {jupyter} jupyter notebook - -ip 0.0.0.0 - -allow-root`

#### 방법 2 : jupyter-notebook container
> `$ docker run -it -d - -name {jupyter} - -gqus all -p 8888:8888 -v $(pwd):/workspace {jupyter-image:tag}`

> `$ docker logs -f {jupyter}`

#### Finally 공통
> 컨테이너 실행 후 jupyter-notebook URL 접속 및 token 입력

### Docker 컨테이너 사용자 계정 추가

#### Docker container를 root 이외의 사용자로 사용하고 싶을 때

##### 1. root 계정의 일반 컨테이너 실행
> `$ docker run -it <image name> bash`

##### 2. 실행한 컨테이너에 새로운 사용자 정보 등록
> `$ adduser <user name>`

> `$ exit`

##### 3. 사용자 계정을 추가한 컨테이너를 Docker Image로 빌드
> `$ docker commit <container id> <new image name>`

##### 4. 해당 Docker Image로 컨테이너 생성
> `$ docker run -it -d --gpus all --name <container name> -u <user name> <new image name>`

##### 5. 컨테이너 실행
> `$ docker exec -it <new image name> bash`
> ***jhi@96a737f944ac:/$***

### Dockerfile

#### Dockerfile 작성
> Dockerfile 작성 시 파일 이름은 반드시 “Dockerfile” 이어야 한다.

> `$ mkdir {apache-dockerfile} && cd {apache-dockerfile}`

> `$ vi Dockerfile`

#### Dockerfile 명령어

- FROM
- 베이스 이미지
- {base-image:tag}
- MAINTAINER
- Dockerfile 작성자
- {작성자 <메일 주소>}
- LABEL
- 이미지 메타데이터 추가 (Key-Value 형태)
- RUN
- 새로운 레이어에서 명령어 실행 && 새로운 이미지 생성
- install -y : 무조건 설치 가능
- WORKDIR
- 작업 디렉토리 지정
- EXPOSE
- Dockerfile 이미지에서 열어줄 포트를 의미  
    컨테이너 생성 시 -p 옵션에 EXPOSE 값을 넣어야 함
- COPY/ADD
- 빌드 명령 도중 호스트의 파일 또는 폴더를 이미지에 가져옴
- 일반적으로는 COPY 사용 권장
- ADD는 압축파일 혹은 네트워크 상의 파일도 가져올 수 있음
- CMD/ENTRYPOINT
- 컨테이너 실행 시 실행할 명령어, 보통 컨테이너 내부에서 항상 돌아가는 서버를 띄울 때 사용
- CMD
- Container를 생성 (docker run) 할 때만 실행
- Container 생성 시 추가적인 명령어에 따라 설정한 명령어를 수정할 수 있음
- $ CMD [“{command}”, “{parameter 1}”, “{parameter 2}”]
- $ CMD {command} {parameter 1} {parameter 2}

- ENTRYPOINT
- Container를 시작 (docker start) 할 때마다 실행
- 컨테이너 시작 시 추가적인 명령어 여부와 관계없이 무조건 실행
- $ ENTRYPOINT [“{command}”, “{parameter 1}”, “{parameter 2}”]
- $ ENTRYPOINT {command} {parameter 1} {parameter 2}

#### Dockerfile로 Docker Image Build

$ docker build -t {image-name:tag} {Dockerfile-path}

$ docker images 명령어로 이미지 생성 확인

### Docker Compose

- 단일 서버에서 여러 개의 Container를 하나의 서비스로 정의해 Bundle로 관리할 수 있도록 작업 환경을 제공하는 관리 도구
    
- Container 수와 정의해야 할  옵션이 많을 때, Docker Compose를 사용해 한번에 묶어서 작업환경을 구축한다.
    
- Docker Compose의 장점 : 하나의 정의로 여러 곳에서 동일 동작을 보장한다.
    

  

#### Docker Compose 설치

1. 설치
    

$ sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname - s)-$(uname -m)" -o /usr/local/bin/docker-compose

2. 권한 부여
    

$ sudo chmod +x /usr/local/bin/docker-compose

3. 백업 파일 이동
    

$ sudo mv /usr/bin/docker-compose /usr/bin/docker-compose.backup

4. 심볼릭 링크 설정
    

$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

5. 버전 확인 (최신 docker는 compose를 내장한다!)
    

$ docker-compose --version

  

#### Docker Compose .yml 작성

기존 Docker run 명령어를 YAML로 변환하는 방식

-> docker compose는 탭을 인식하지 못 하기 때문에, YAML 파일 들여쓰기 시 공백 2개로 구분

- version
    

- YAML 파일 포맷 버전, Docker Compose 지원 최신 버전으로 설정
    
- version 1 : 버전 생략
    
- version 2 : 마이너 버전 설정 필요(2.x), 마이너 버전 생략 시 2.0 적용
    
- version 3 : 도커 스웜과 함께 사용하도록 디자인
    

- services
    

- 생성될 Container들을 묶는 단위
    

- command
    

- 백그라운드 종료 방지, sleep infinity로 설정
    

- image
    

- Service Container를 생성할 때 사용할 이미지
    

- environment
    

- docker 명령어 -e 옵션과 동일, Service Container 내부에서 사용할 환경 변수 지정
    

- volumes
    

- docker 명령어 -v 옵션과 동일, 호스트와 컨테이너 디렉토리 바인딩
    

- depends_on
    

- 특정 컨테이너에 대한 의존 관계, 명시한 컨테이너 먼저 생성한 후 실행
    

  

#### Docker Compose 명령어

- Docker Compose 명령어는 원래 $ docker-compose 이지만, docker 버전 업데이트 이후 $ docker compose 가 가능해짐
    
- 프로젝트 실행
    

- yml 파일 필요
    
- $ docker compose -p {project-name} up -d
    
- -d
    

- Docker Compose Project를 background에서 실행
    

- -p {project-name}
    

- 프로젝트 이름을 변경하여 실행
    

- --force-recreate
    

- 컨테이너를 지우고 새로 생성
    

- --build
    

- 서비스 시작 전 이미지를 새로 생성
    

- --env-file
    

- 환경변수 파일 지정
    

- -f
    

- 기본으로 제공하는 docker-compose.yml이 아닌 별도의 파일명 실행
    
- $ docker compose -f docker-compose.yml -f docker-compose-test.yml up
    

- 두 개의 YAML 파일 실행
    
- YAML 파일을 두 개 이상 설정하는 경우 뒤에 오는 설정 파일이 우선됨
    

- 프로젝트 종료
    

- $ docker compose down -v
    
- -v or --volume
    

- 프로젝트 종료 시 네트워크, 볼륨 제거
    
- 모든 설정을 초기화하고 새로 시작할 때 사용한다.
    
- DB 데이터 초기화에 용이하다.
    

- 서비스 시작
    

- $ docker compose start
    

- 서비스 멈춤
    

- $ docker compose stop
    

- 프로젝트 목록 확인
    

- 현재 환경에서 실행 중인 서비스 각각의 상태를 표시한다.
    
- $ docker compose ps
    

- 프로젝트 로그 확인
    

- $ docker compose logs
    
- -f or --follow
    

- 실시간 로그 출력
    

- 프로젝트 Container 명령어 실행
    

- $ docker compose exec {service} {command}
    

- 프로젝트 Container 명령어 일회성 실행
    

- $ docker compose run {service} {command}
    
- --rm
    

- 명령어 종료 후 컨테이너 종료
    

  

|   |   |
|---|---|
|$ run|$ exec|
|- 실행 시 새로운 컨테이너를 띄움<br>    <br>- batch 성 작업에 특화|- 실행되어 있는 컨테이너에 접속<br>    <br>- 프로세서를 실행시켜 놓을 때 사용|

  

- 프로젝트 구성 확인
    

- Docker Compose에 최종 적용된 설정 확인
    
- 보통 여러 개의 YAML 파일을 실행한 경우 최종 설정값을 확인하기 위해 사용
    
- $ docker compose config
    

  

#### Docker Compose .env

- Docker Compose에도 각 실행 환경에 따라 변경되어야 하는 옵션들이 있을 수 있다.  
    이때, 파일을 일일이 수정하는 건 비효율적
    
- 따라서, 실행 환경마다 환경변수 파일을 정의하여 동일한 Compose 파일로 각 환경에서 동적인 옵션 실행이 가능하게 한다.
    
- .env file
    

1. Docker Compose Project를 실행 (up) 한 상태에서,
    
2. $ cat .env
    
3. $ cat docker-compose.yml
    
4. $ docker-compose config
    

  
**