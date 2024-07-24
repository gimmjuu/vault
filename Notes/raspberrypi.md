---
created:
  2024-06-24
edited:
  2024-07-03
tags:
  - BOARD
  - RPI
---

<link rel="stylesheet" href="../Assets/style.css" />

# Raspberry Pi 5

## [Getting started](https://www.raspberrypi.com/documentation/computers/getting-started.html)

1. [Raspberry Pi 5 + Active Cooler + AI Kit 조립](https://m.blog.naver.com/PostView.naver?blogId=icbanq&logNo=223382909813&fromRecommendationType=category&targetRecommendationDetailCode=1000)
  - Connection: DVC 전원 + HDMI + Monitor + Keyboard + mouse

2. [Operating System Installation](https://www.raspberrypi.com/software/)
  - Choose raspberry pi device version
  - Choose operating system(ex. Raspberry Pi OS 64-bit)
  - Choose storage(Micro SD recommanded)
  - OS Customisation

3. Setup Rpi board

<details>
  <summary class="_pinksmall">Docker for Rpi 5</summary>

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
</details>

## Basic Tutorials

- 스크래치 <scan class="_yellowsmall">Scratch</scan>
  - 시각적 프로그래밍 도구
  - Drag&Drop interface 제공
  - 애니메이션 및 게임 제작
- 파이썬<scan class="_yellowsmall">Python</scan>
- Sonic Pi<scan class="_yellowsmall">Sonic Pi</scan>
- 터미널<scan class="_yellowsmall">Terminal</scan>
- GPIO<scan class="_yellowsmall">GPIO</scan>
- 마인크래프트<scan class="_yellowsmall">Minecraft</scan>
- 파이썬 게임<scan class="_yellowsmall">Python games</scan>
- 워드프레스<scan class="_yellowsmall">WordPress</scan>
- 매스매티카<scan class="_yellowsmall">Mathematica</scan>
  - 과학, 수학, 컴퓨팅 및 공학에서 사용되는 계산 프로그래밍 도구
- 카메라 모듈<scan class="_yellowsmall">Camera module</scan>
- 웹캠<scan class="_yellowsmall">Webcam</scan>
- 코디<scan class="_yellowsmall">Kodi</scan>
- 오디오 재생<scan class="_yellowsmall">Audio</scan>
- 비디오 재생<scan class="_yellowsmall">Video</scan>
- 데모 프로그램<scan class="_yellowsmall">Demos</scan>

### 카메라 모듈

- 플렉스 케이블을 커넥터에 삽입

#### 카메라 사용 설정

- `raspi-config` 도구 실행

```bash
sudo raspi-config
```

- `Enable camera` - `Enter` - `Finish` - `Reboot`

#### 카메라 사용

1. Shell 카메라 모듈을 사용하기 위한 명령행 도구 `raspicam`

<details>
  <summary class="_pinksmall">1. raspistill</summary>

- 카메라 모듈을 사용하여 정지 사진을 캡쳐하기 위한 명령행 도구
- 카메라 모듈은 `2592 x 1944` 해상도로 사진을 저장, 2.4MB

- 기본 사용

```bash
raspistill -o Desktop/cam.jpg
```

- 수직/수평 뒤집기
  - vf : vertical flip
  - hf : horizontal flip

```bash
raspistill -vf -hf -o cam2.jpg
```

- Bash Script
1. `camera.sh`

```bash
#!/bin/bash

DATE=$(date + "%Y-%m-%d_%H%M")

raspistill -vf -hf -o /home/pi/camera/$DATE.jpg
```

2. 스크립트 실행

```bash
mkdir camera

chmod +x camera.sh

./camera.sh
```

3. 가능한 옵션 전체 목록을 스크롤 출력

```bash
raspistill 2>&1 | less
```

</details>

<details>
  <summary class="_pinksmall">2. raspivid</summary>

- 카메라 모듈을 사용하여 비디오를 캡쳐하기 위한 명령행 도구
- 기본 사용

```bash
raspivid -o vid.h264
```

- `-t` 플래그 : 비디오의 길이를 밀리초 숫자로 전달

```bash
raspivid -o video.h264 -t 10000
```

- 가능한 옵션 전체 목록을 스크롤 출력

```bash
raspivid 2>&1 | less
```

- MP4 Video format

```bash
sudo apt-get install -y gpac
```

```bash
# Capture 30 seconds of raw video at 640x480 and 150kb/s bit rate into a pivideo.h264 file:
raspivid -t 30000 -w 640 -h 480 -fps 25 -b 120000 -p 0,0,640,480 -o pivideo.h264
# Wrap the raw video with an mp4 contailne;
MP4Box -add pivideo.h264 pivideo.mp4
# Remove the source raw file, leaving the ramaining pivideo.mp4 file to play
rm pivideo.h264
```
- Wrap MP4 around your existing raspivid output

```bash
MP4Box -add video.h264 video.mp4
```
</details>

<details>
  <summary class="_pinksmall">3. timelapse</summary>

1. Raspistill 타임랩스 모드
  - `--timelapse` 또는 `-tl` 밀리초 단위 설정
  - `%04d` 파일명에 프레임 번호 삽입

```bash
raspistill -t 30000 -tl 2000 -o image%04d.jpg 
```

2. cron
  - 사진을 주기적으로 촬영하는 방법
  - cron 테이블 편집

```bash
crontab -e
```

```bash
* * * * * /home/pi/camera.sh 2>&1
```

```bash
cromtab: installing new crontab
```

3. 이미지 이어붙이기
  - 사진을 이어붙여 비디오 생성

- 3-1. 이미지 파일을 데스크톱 컴퓨터 또는 노트북으로 옮겨 비디오 처리

```bash
ls *.jpg > stills.txt
```

- 3-2. 라즈베리파이 `avconv`
  - 라즈베리 파이 하드웨어 가속 없이 소프트웨어를 사용하여 인코딩하므로 속도 면에서 비효율적
  - avconv 설치 후 jpeg 파일들을 h264 비디오 파일로 변환

```bash
sudo apt-get install libav-tools

avconv -r 10 -i image%04d.jpg -r 10 -vcodec libx264 -vf scale=1280:720 timelapse.mp4
```
- ✔
  - `-r 10` 입력과 출력 파일이 초당 10 프레임인 것으로 가정
  - `-i image%04.jpg` 입력 파일 사양(캡쳐하는 동안 생성된 파일과 일치하기 위함)
  - `-vcodec libx264` 소프트웨어 x264 인코더를 사용
  - `-vf scale=1280:720` 720p로 스케일
    - 필요에 따라 1920:1080 또는 그보다 낮은 해상도를 사용할 수 있음
    - 라즈베리 파이에서는 1080p 비디오까지만 재생할 수 있지만, 4K로 재생하고자 한다면 설정할 수 있음
  - `timelapse.mp4` 출력 파일명
- avconv는 다양한 인코딩 옵션 및 기타 설정을 위한 포괄적인 파라미터를 제공함

- 3-3. 리눅스 대체 패키지 `mencoder`

```bash
sudo apt-get install mencoder
```

```bash
mencoder -nosound -ovc lavc -lavcopts vcodec=mpeg4:aspect=16/9:vbitrate=8000000 -vf scale=1920:1080 -o timelapse.avi -mf type=jpeg:fps=24 mf://@stills.txt
```
</details>

<details>
  <summary>4. raspiyuv</summary>

- `raspiyuv`는 `raspistill`과 같은 기능이지만, 표준 이미지 파일 대신 카메라로부터 가공을 거치지 않은 raw 이미지 파일을 생성
</details>

2. Python `picamera`

### 표준 USB 웹캠

- `fswebcam` 패키지 설치

```bash
sudo apt-get install fswebcam
```

- 파일명을 지정하여 웹캠 촬영 이미지를 저장

```bash
fswebcam image.jpg
```
```bash
--- Opening /dev/video0...
Trying source module v412...
/dev/video0 opened.
No input was specified, using the first.
Adjusting resolution from 384x288.
--- Capturing frame...
Corrupt JPEG data: 2 extraneous bytes before marker 0xd4
Captured frame in 0.00 seconds.
--- Processing captured image...
Writing JPEG image to 'image.jpg'.
```

- `-r` 웹캠의 기본 해상도를 변경 지정

```bash
fswebcam -r 1280x720 image2.jpg
```
```bash
--- Opening /dev/video0...
Trying source module v412...
/dev/video0 opened.
No input was specified, using the first.
--- Capturing frame...
Corrupt JPEG data: 1 extraneous bytes before marker 0xd5
Captured frame in 0.00 seconds.
--- Processing captured image...
Writing JPEG image to 'image2.jpg'
```

- `--no-banner` 배너 숨기기

```bash
fswebcam -r 1280x720 --no-banner image3.jpg
```
```bash
--- Opening /dev/video0...
Trying source module v412...
/dev/video0 opened.
No input was specified, using the first.
--- Capturing frame...
Corrupt JPEG data: 2 extraneous bytes before marker 0xd6
Captured frame in 0.00 seconds.
--- Processing captured image...
Disabling banner.
Writing JPEG image to 'image3.jpg'.
```

- 저품질 사진
  - 웹캠은 라즈베리파이 카메라 모듈에 비해 신뢰성이 떨어짐
  - 저품질 사진이 찍힐 수 있음

- Bash Script
  - 웹캠으로 사진을 찍는 Bash 스크립트

```bash
mkdir webcam
```

```bash
#!/bin/bash

DATE=$(data+"%Y-%m-%d_%H%M")

fswebcam -r 1280x720 --no-banner /home/pi/webcam/$DATE.jpg
```

```bash
chmod +x webcam.sh

./webcam.sh
```
```bash
--- Opening /dev/video0...
Trying source module v412...
/dev/video0 opened.
No input was specified, using the first.
--- Capturing frame...
Corrupt JPEG data: 2 extraneous bytes before marker 0xd6
Captured frame in 0.00 seconds.
--- Processing captured image...
Disabling banner.
Writing JPEG image to '/home/pi/webcam/2013-06-07_2338.jpg'.
```

- cron을 사용한 타임 랩스
  - 사진을 주기적으로 타임랩스를 캡쳐

```bash
crontab -e
```
```bash
* * * * * /home/pi/webcam.sh 2>&1
```
```bash
crontab: installing new crontab
```

- 유틸리티
  - SSH
    - 로컬 네트워크를 통해 라즈베리 파이에 원격으로 접근하기 위해 SSH 사용
  - SCP
    - 파이로 찍은 사진 파일을 SSH를 통해 주 컴퓨터로 복사
  - rsync
    - `rsync`를 사용하여 파이와 컴퓨터의 사진 폴더를 동기화
  - cron
    - `cron`을 사용하여 일정한 주기로 사진 촬영

### 코디

- 라즈베리 파이에서 구동되는 미디어 센터 소프트웨어
- NOOBS에는 코디 배포판 LibreELEC과 OSMC가 포함되어 있음

#### `NOOBS`

- NOOBS 운영 체제 중 LibreELEC 또는 OSMC를 설치

```bash
OS(es) Installed Successfully
```

#### 코디 사용하기

- 코디 배포판을 사용해 미디어 파일을 재생하고, 시스템을 구성하고 애드온을 설치하여 기능을 추가함

### 오디오 재생

- MP3 파일 재생

```bash
omxplayer example.mp3
```

- 예제 파일 다운로드

```bash
wget http://rpf.io/lamp3 -O example.mp3 --no-check-certificate
```

- HDMI 출력 강제

```bash
omxplayer -o hdmi example.mp3
```

- 헤드폰 잭 출력 강제

```bash
omxplayer -o local example.mp3
```

- 헤드폰 잭과 HDMI 동시 출력

```bash
omxplayer -o both example.mp3
```

### 비디오 재생

- Raspberry Pi에서 비디오 재생

```bash
omxplayer example.mp4
```

- Pi 4에서는 MPEG2 및 VC-1 코덱에 대한 하드웨어 지원이 제거됨
  - VLC 응용 프로그램을 사용

- 비디오 샘플 Big Buck Bunny

```bash
omxplayer /opt/vc/src/hello_pi/hello_video/test.h264
```

```bash
vlc /opt/vc/src/hello_pi/hello_video/test.h264
```

- VLC를 사용할 때 Raspberry Pi 카메라 모듈 등 원시 H264 스트림을 캡슐화 하여 재생 성능을 향상시킬 수 있음
  - 30 fps에서 `video.h264`를 컨테이너화된 `video.mp4`로 변환

```bash
ffmpeg -r 30 -i video.h264 -c:v copy video.mp4
```

## Hailo project

- [hailo-rpi5-examples](https://github.com/hailo-ai/hailo-rpi5-examples?tab=readme-ov-file)
  1. [Setup rpi and hailo](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/install-raspberry-pi5.md#how-to-set-up-raspberry-pi-5-and-hailo-8l)
  2. [Basic Pipelines](https://github.com/hailo-ai/hailo-rpi5-examples/blob/main/doc/basic-pipelines.md#hailo-rpi5-basic-pipelines)

### HailoRT-Automation

- `hailo_tracker.cpp`

```cpp
// Tracker Includes
#include "jde_tracker/jde_tracker.hpp"

#include "hailo_tracker.hpp"
#include "hailo_common.hpp"

std::mutex HailoTracker::mutex_;

class HailoTracker::HailoTrackerPrivate
{
public:
    std::map<std::string, JDETracker> tracker;
};

HailoTracker::HailoTracker() : priv(std::make_unique<HailoTrackerPrivate>()){};
HailoTracker::~HailoTracker(){};
HailoTracker &HailoTracker::GetInstance()
{
    std::lock_quard<std::mutex> lock(mutex_);
    static HailoTracker instance;
    return instance;
}

void HailoTracker::remove_jde_tracker(const std::string &name)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers.erase(name);
}

std::vector<std::string> HailoTracker::get_trackers_list()
{
    std::lock_guard<std::mutex> lock(mutex_);
    std::vector<stc::string> trackers_list;
    for (auto &tracker : priv->trackers)
    {
        trackers_list.push_back(tracker.first);
    }
    return trackers_list;
}

void HailoTracker::add_jde_tracker(const std::string &name, HailoTrackerParams tracker_params)
{
    std::lock_guard<std::mutex> lock(mutex_);

    priv->trackers.emplace(std::piecewise_construct, std::forward_as_tuple(name),
                           std::forward_as_tuple(tracker_params.kalman_distance,
                                                 tracker_params.iou_threshold,
                                                 tracker_params.init_iou_threshold,
                                                 tracker_params.keep_tracked_frames,
                                                 tracker_params.keep_new_frames,
                                                 tracker_params.keep_lost_frames,
                                                 tracker_params.keep_past_metadata,
                                                 tracker_params.std_weight_position,
                                                 tracker_params.std_weight_position_box,
                                                 tracker_params.std_weight_velocity,
                                                 tracker_params.std_weight_velocity_box,
                                                 tracker_params.debug,
                                                 tracker_params.hailo_objects_blacklist));
}

void HailoTracker::add_jde_tracker(const std::string &name)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers.emplace(name, JDETracker());
}

std::vector<HailoDetectionPtr> HailoTracker::update(const std::string &name, std::vector<HailoDetectionPtr> &inputs)
{
    std::lock_guard<std:mutex> lock(mutex_);
    priv->trackers.emplace(name, JDETracker());
}

std::vector<HailoDetectionPtr> HailoTracker::Update(const std::string &name, std::vector<HailoDetectionPtr> &inputs)
{
    std::lock_guard<std::mutex> lock(mutex_);
    auto online_stracks = priv->trackers[name].update(inputs);
    bool debug = priv->trackers[name].get_debug();
    return JDETracker::stracks_to_hailo_detections(online_stracks, debug);
}

void HailoTracker::add_object_to_track(const std::string &name, int track_id, HailoObjectPtr obj)
{
    std::lock_guard<std::mutex> lock(mutex_);
    STrack *tracked_detection = priv->trackers[name].get_detection_with_id(track_id);
    if (nullptr != tracked_detection)
    {
        tracked_detection->add_object(obj);
    }
}

void HailoTracker::remove_matrices_from_track(const std::string &name, int track_id)
{
    std::lock_guard<std::mutex> lock(mutex_);
    STrack *tracked_detection = priv->trackers[name].get_detection_with_id(track_id);
    if (tracked_detection)
    {
        std::vector<HailoObjectPtr> matrices;
        auto detection = tracked_detection->get_hailo_detection();
        for (auto obj : detection->get_objects_typed(HAILO_MATRIX))
        {
            HailoMatrixPtr matrix = std::dynamic_pointer_cast<HailoMatrix>(obj);
            matrices.push_back(matrix);
        }
        hailo_common::remove_objects(detection, matrices);
    }
}

void HailoTracker::remove_classifications_from_track(const std::string &name, int track_id, std::string classifier_type)
{
    std::lock_guard<std::mutex> lock(mutex_);
    STrack *tracked_detection = pri->trackers[name].get_detection_with_id(track_id);
    if (tracked_detection)
    {
        hailo_common::remove_classifications(tracked_detection->get_hailo_detection(), classifier_type);
    }
}

// Setters for members accessible at element-property level
void HailoTracker::set_kalman_distance(const std::string &name, float new_distance)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_kalman_distance(new_distance);
}

void HailoTracker::set_iou_threshold(const std::string &name, float new_iou_thr)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_iou_threshold(new_iou_thr);
}

void HailoTracker::set_init_iou_threshold(const std::string &name, float new_init_iou_thr)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_init_iou_threshold(new_init_iou_thr);
}

void HailoTracker::set_keep_tracked_frames(const std::string &name, int new_keep_tracked)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_keep_tracked_frames(new_keep_tracked);
}

void HailoTracker::set_keep_new_frames(const std::string &name, int new_keep_new)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackes[name].set_keep_new_frames(new_keep_new);
}

void HailoTracker::set_keep_lost_frames(const std::string &name, int new_keep_lost)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_keep_lost_frames(new_keep_lost);
}

void HailoTracker::set_keep_past_meradata(const std::string &name, bool new_keep_past)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_keep_past_metadata(new_keep_past);
}

void HailoTracker::set_std_weight_position(const std::string &name, float new_std_weight_pos)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_std_weight_position(new_std_weight_pos);
}

void HailoTracker::set_std_weight_position_box(const std::string &name, float new_std_weight_position_box)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_std_weight_position_box(new_std_weight_position_box);
}

void HailoTracker::set_std_weight_velocity(const std::string &name, float new_std_weight_vel)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_std_weight_velocity(new_std_weight_vel);
}

void HailoTracker::set_std_weight_velocity_box(const std::string &name, float new_std_weight_velocity_box)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_std_weight_velocity_box(new_std_weight_velocity_box);
}

void HailoTracker::set_debug(const std::string &name, bool new_debug)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_debug(new_debug);
}

void HailoTracker::set_hailo_objects_blacklist(const std::string &name, std::vector<hailo_object_t> hailo_objects_blacklist_vec)
{
    std::lock_guard<std::mutex> lock(mutex_);
    priv->trackers[name].set_hailo_objects_blacklist(hailo_objects_blacklist_vec);
}
```

- `hailo_tracker.hpp`

```cpp
#pragma once

// General cpp includes
#include <iostream>
#include <vector>
#include <mutex>
#include <thread>
#include <map>

#include "hailo_objects.hpp"

#define DEFAULT_KALMAN_DISTANCE (0.7f)
#define DEFAULT_IOU_THRESHOLD (0.8f)
#define DEFAULT_INIT_IOU_THRESHOLD (0.9f)
#define DEFAULT_KEEP_FRAMES (2)
#define DEFAULT_KEEP_PAST_METADATA (true)
#define DEFAULT_STD_WEIGHT_POSITION (0.01)
#define DEFAULT_STD_WEIGHT_POSITION_BOX (0.00000001)
#define DEFAULT_STD_WEIGHT_VELOCITY (0.001)
#define DEFAULT_STD_WEIGHT_VELOCITY_BOX (0.00000001)
#define DEFAULT_DEBUG (false)
#define DEFAULT_HAILO_OBJECTS_BLACKLIST                       \
    {                                                         \
        HAILO_LANDMARKS, HAILO_DEPTH_MASK, HAILO_CLASS_MASK   \
    }

struct HailoTrackParams
{
    float kalman_distance;
    float iou_threshold;
    float init_iou_threshold;
    int keep_tracked_frames;
    int keep_new_frames;
    int keep_lost_frames;
    bool keep_past_metadata;
    float std_weight_position;
    float std_weight_position_box;
    float std_weight_velocity;
    float std_weight_velocity_box;
    bool debug;
    std::vector<hailo_object_t> hailo_objects_blacklist;
};

class HailoTracker
{
private:
    class HailoTrackerPrivate;
    std::unique_ptr<HailoTrackerPrivate> priv;
    HailoTracker(const HailoTracker &) = delete;
    HailoTracker &operator=(const HailoTracker &) = delete;
    ~HailoTracker();
    HailoTracker();
    static std::mutex mutex_;

public:
    static HailoTracker &GetInstance();
    void add_jde_tracker(const std::string &name, HailoTrackerParams params);
    void add_jde_tracker(const std::string &name);
    void remove_jde_tracker(const std::string &name);
    std::vector<std::string> get_trackers_list();
    std::vector<HailoDetectionPtr> update(const std::string &name, std::vector<HailoDetectionPtr> &inputs);
    void add_object_to_track(const std::string &name, int id, HailoObjectPtr obj);
    void remove_classifications_from_track(const std::string &name, int track_id, std::string classifier_type);
    void remove_matrices_from_track(const std::string &name, int track_id);

    // Setters for members accessible at element-property level
    void set_kalman_distance(const std::string &name, float new_distance);
    void set_iou_threshold(const std::string &name, float new_iou_thr);
    void set_init_iou_threshold(const std::string &name, float new_init_iou_thr);
    void set_keep_tracked_frames(const std::string &name, int new_keep_tracked);
    void set_keep_new_frames(const std::string &name, int new_keep_new);
    void set_keep_lost_frames(const std::string &name, int new_keep_lost);
    void set_keep_past_metadata(const std::string &name, bool new_keep_past);
    void set_std_weight_position(const std::string &name, float new_std_weight_pos);
    void set_std_weight_position_box(const std::string &name, float new_std_weight_position_box);
    void set_std_weight_velocity(const std::string &name, float new_std_weight_vel);
    void set_std_wdight_velocity_box(const std::string &name, float new_std_weight_velocity_box);
    void set_debug(const std::string &name, bool new_debug);
    void set_hailo_objects_blacklist(const std::string &name, std::vector<hailo_object_t> hailo_objects_blacklist_vec);
};
```

- `meson.build`

```sh
tracker_sources = ['hailo_tracker.cpp']

#########################################
# Hailo Tracker Shared Library
#########################################
tracker_lib = shared_library('hailo_tracker',
    tracker_sources,
    c_args : hailo_lib_args,
    cpp_args : hailo_lib_args,
    include_directories : hailo_general_inc + [xtensor_inc],
    dependencies : [opencv_dep],
    gnu_symbol_visibility : 'default',
    version : meson.project_version(),
    install : true,
    install_dir : get_option('libdir'),
)

tracker_dep = declare_dependency(
    include_directories : [include_directories('.')],
    link_with : tracker_lib)

if not get_option('include_python')
    subdir_done()
endif

#########################################
# Hailo Tracker Python Extension Module
#########################################
tracker_pybind_sources = [
    'jde_tracker/python_bindings/tracker_pybindmain.cpp'
]

python_installation.extension_module('pyhailotracker',
    tracker_pybind_sources,
    include_directories : [hailo_general_inc, xtensor_inc, include_directories('./jde_tracker')],
    dependencies : [opencv_dep, pybind_dep],
    link_language : 'cpp',
    override_options : ['cpp_rtti=true',],
    gnu_symbol_visibility : 'default',
    install : true,
    install_dir : python_package_install_dir,
)
```

- `hailo_common.hpp`

```cpp
/* __BEGIN_DECLS should be used at the beginning of your declarations, so that C++ compilers don't mangle their names. Use __END_DECLS at the end of C declarations. */

#pragma once
#include "hailo_objects.hpp
// #include <stdlib.h>
// #include <string>
// #include <cstring>

#undef __BEGIN_DECLS
#undef __END_DECLS
#ifdef __cplusplus
#define __BEGIN_DECLS \
    extern "C"        \
    {
#define __END_DECLS }
#else
#define __BEGIN_DECLS /* empty */
#define __END_DECLS   /* empty */
#endif

namespace hailo_common
{
    inline void add_object(HailoROIPtr roi, HailoObjectPtr obj)
    {
        roi->add_object(obj);
    }

    inline void add_objects(HailoROIPtr roi, std::vector<HailoObjectPtr> objects)
    {
        for (HailoObjectPtr obj : objects)
        {
            add_object(roi, obj);
        }
    }

    inline void add_classification(HailoROIPtr roi, std::string type, std::string label, float confidence, int class_id = NULL_CLASS_ID)
    {
        add_object(roi,
                   std::make_shared<HailoClassification>(type, class_id, label, confidence));
    }

    inline HailoDetectionPtr add_detection(HailoROIPtr roi, HailoBBox bbox, std::string label, float confidence, int class_id = NULL_CLASS_ID)
    {
        HailoDetectionPtr detection = std::make_shared<HailoDetection>(bbox, class_id, label, confidence);
        detection->set_scaling_bbox(roi->get_bbox());
        add_object(roi, detection);
        return detection;
    }

    inline void add_detections(HailoROIPtr roi, std::vector<HailoDetection> detections)
    {
        for (auto det : detections)
        {
            add_object(roi, std::make_shared<HailoDetection>(det));
        }
    }

    inline void add_detection_pointers(HailoROIPtr roi, std::vector<HailoDetectionPtr> detections)
    {
        for (auto det : detections)
        {
            det->set_scaling
        }
    }
}
```

## Reference

[libcamera connection](https://fishpoint.tistory.com/9288)
[remote connect](https://velog.io/@thdusdl4767/Raspberry-Pi-Visual-Studio-Code%EC%97%90%EC%84%9C-%EB%9D%BC%EC%A6%88%EB%B2%A0%EB%A6%AC%ED%8C%8C%EC%9D%B4-%EC%9B%90%EA%B2%A9%EC%A0%9C%EC%96%B4)

- Additional Tutorial

[How to build a Raspberry Pi NAS](https://www.raspberrypi.com/tutorials/nas-box-raspberry-pi-tutorial/)
