---
created:
  2024-06-20
tags:
  - API
---

<link rel="stylesheet" href="../Assets/style.css" />

<span class="_big _600">🔖Index</span><br>

- [1. Gstreamer?](##gstreamer)
- [2. Gstreamer 기본 개념](##gstreamer-기본-개념)
- [3. 3 Types of Elements](##3-types-of-elements)

---
# Gstreamer

## `Gstreamer`?

- 스트리밍 미디어 응용 프로그램을 만들기 위한 프레임워크
    * 모든 유형의 스트리밍 멀티미디어 응용 프로그램을 작성할 수 있음
    * 구성 요소를 임의의 파이프라인에 혼합해 응용 프로그램을 작성할 수 있음
- <span class="_yellowsmall">"gStreamer is a multimedia tool that connects a sequence of elements through a pipe line"</span>

## Gstreamer 기본 개념

### 구성요소

- `elements`
    * 가장 중요한 객체 클래스
    * 서로 연결된 Chain of Elements를 만들고, 이를 통해 데이터가 흐름
- `pads`
    * Element의 입력, 출력
    * 다른 elements와 연결할 수 있음
    * 흐르는 데이터 유형 제한 가능
    * 두 패드 간 허용된 데이터 유형이 호환되는 경우에만 링크 허용
        - 패드 == 물리적 잭
    * 대부분의 데이터는 한방향으로 흐름
        - source pads 출력 `output`
        - sink pads 입력 `input`
- `bin`
    * Elements의 컨테이너
    * bin의 상태를 변경해 하위 모든 요소의 상태를 변경
- `pipeline`
    * 최상위 bin
    * 응용 프로그램을 위한 버스 제공
    * 하위 요소에 대한 동기화 관리

<figure>
    <img src="../Assets/Images/" alt="bin & pipeline">
    <figcaption>[출처] Medium | [Gstreamer] Gstreamer 기초 | https://medium.com/may-i-lab/gstreamer-gstreamer-%EA%B8%B0%EC%B4%88-da5015f531fc</figcaption>
</figure>

### 통신 Communication

    애플리케이션-파이프라인 간 통신 및 데이터 교환을 위한 메커니즘 제공

- `buffer`
    * 파이프라인 요소 간 데이터 전달을 위한 객체
    * source에서 sink로 이동
- `event`
    * 요소에서 요소 또는 응용 프로그램에서 요소로 전송되는 객체
- `message`
    * 파이프라인 메시지 버스에 요소 별로 게시된 객체
    * 응용 프로그램에서 수집을 위해 유지함
- `query`
    * 응용 프로그램이 query를 통해 파이프라인에서 지속시간 현재 재생 위치 등 정보를 요청

## 3 Types of Elements

    element는 source element -> filter | demuxer -> sink element 로 연결된다.

- `source elements`
    * 디스크, 사운드 카드에서 읽어 파이프라인에서 사용할 데이터 생성
    * 입력 없음, 오직 출력만

- `filters`, `convertors`, `demuxers`, `muxers` and `codecs`
    * 입력, 출력 pad
        + 데이터를 받고, 처리한 후 내보냄
    * 1:1
        + filter: 볼륨
        + convertor: 비디오 크기 변환
    * 1:N
        + demuxer, muxer
    * ogg: 비디오&음성 포함 파일

- `sink elements`
    * 미디어 파이프라인의 끝점
    * 입력만 있고 출력이 없음
    
## bins

- `bin`
    * container element
    * 여러 element를 감싼 컨테이너
- element들의 상태를 한번에 관리함
    * NULL      기본 상태
    * READY     준비 상태
    * PAUSED    정지 상태
    * PLAYING   재생 상태

## Gstreamer Tutorial

    pipeline 구성 예제

### basic elements

- 애니메이션 비디오 팬턴 창 출력

```bash
gst-launch-1.0 videotestsrc ! videoconvert ! autovideosink
```

### properties

- videotestsrc에 pattern을 부여하는 property
    * result: 원 출력

```bash
gst-launch-1.0 videotestsrc pattern=11 ! videoconvert ! autovideosink
```

### pads

- elements 연결 시 사용할 패드를 직접 지정할 수 있음
- 영상과 음성이 합쳐진 영상의 url
    * `matroskademux` video만 가져옴
    * `matroskamux` 병합
    * sintel_video.mkv로 저장

```bash
gst-launch-1.0 souphttpsrc location=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! matroskademux name=dd.video_0 ! matroskamux ! filesink location=sintel_video.mkv
```

- 오디오만 가져오고 싶은 경우

```bash
gst-launch-1.0 souphttpsrc location=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! matroskademun ! video/x-vp8 ! matroskamux ! filesink location=sintel_video.mkv
```

### caps filters 사용

- 요소에 둘 이상의 출력 패드가 있는 경우 모든 패드와 호환 가능
    * 그중 가능한 첫번째 필드를 사용해 링크하는 방법
- 패드가 video_0에 연결될 지 audio_0에 연결될 지 모르는 상황?
    * 제대로 저장되지 않을 수 있음

```bash
gst-launch-1.0 souphttpsrc location=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! matroskademux ! filesink location=test
```

- pads 사용 방법과 동일한 영상 저장을 위해서 명명 패드를 사용하거나 caps filter 사용
- video/x_vp8을 추가하여 audio_0이 아닌 video_0을 호출
    * pads 사용 시처럼 영상 저장

```bash
gst-launch-1.0 souphttpsrc location=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! video/x-vp8 ! matroskamux ! filesink location=sintel_video.mkv
```

### videoscale

- url 영상의 scale을 변경한 후 convert하여 queue에 할당

```bash
gst-launch-1.0 uridecodebin uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! queue ! videoscale ! video/x-raw-yuv, width=320, height=200 ! videoconvert ! autovideosink
```

### file i/o

- `filesrc` 파일에서 영상 가져오기

```bash
st-launch-1.0 filesrc location=f:\\media\\sintel\\sintel_trailer-480p.webm ! decodebin ! autovideosink
```

- `filesink` 파일로 영상 쓰기

```bash
gst-launch-1.0 audiotestsrc ! vorbisenc ! oggmux ! filesink location=test.ogg
```

### test media creation

- video

```bash
gst-launch-1.0 videotestsrc ! videoconvert ! autovideosink
```

- audio

```bash
gst-launch-1.0 audiotestsrc ! audioconvert ! autoaudiosink
```

### video adapter

- `videoconvert`
    * 색상 공간 변환 like RGB → YUV
- `videorate`
    * 비디오 프레임을 가져와 새로운 스트림 생성 → 속도 변환에 주로 사용

```bash
gst-launch-1.0 videotestsrc ! video/x-raw,framerate=30/1 ! videorate ! video/x-raw,framerate=1/1 ! videoconvert ! autovideosink
```

- `videoscale`
    * 비디오 프레임 크기 조정

```bash
gst-launch-1.0 uridecodebin
uri=https://www.freedesktop.org/software/gstreamer-sdk/data/media/sintel_trailer-480p.webm ! videoscale ! video/x-raw,width=178,height=100 ! videoconvert ! autovideosink
```

### multi threading

- `queue`
- `queue2`
- `multiqueue`
- `tee`

```bash
gst-launch-1.0 audiotestsrc ! tee name=t ! queue ! audioconvert ! autoaudiosink t. ! queue ! wavescope ! videoconvert ! autovideosink
```

## 가속화 플러그인

- `gst-nvvideocodecs`
    * 가속화된 비디오 디코더
- `v4l2h264dec`
    * 가속화 전용 플러그인
- NVIDIA로 가속화된 플러그인
    * `gst-nvvideocodecs`, `gst-nvstreammux`, `gst-nvinfer`, `gst-nvtracker`, `gst-nvosd`, `gst-tiler`, `gst-nvvidconv`

