---
created:
  2024-07-30
tags:
  - STREAM
  - RTSP
---

<link rel="stylesheet" href="../Assets/style.css" />

# Topic

- Stream MP4 via RTSP using FFmpeg

## Prerequisites

- Install FFmpeg

## Stepbystep Guide ver.gpt

1. Prepare the RTSP Server

- 방법 1. Git repository clone

```bash
git clone https://github.com/aler9/rtsp-simple-server
cd rtsp-simple-server
./rtsp-simple-server
```

- 방법 2. Docker image run

<details>
  <summary class="_bgblue _yellowsmall">Available Images</summary>

|Tag Name|FFmpeg|RPI Camera|
|:-:|:-:|:-:|
|bluenviron/mediamtx:latest|x|x|
|bluenviron/mediamtx:latest-ffmpeg|o|x|
|bluenviron/mediamtx:latest-rpi|x|o|
|bluenviron/mediamtx:latest-ffmpeg-rpi|o|o|
</details>

```bash
docker run --rm -it --network=host bluenviron/mediamtx:latest
```

<details>
  <summary class="_bgblue _yellowsmall">--network=host</summary>

The `--network=host` flag is mandatory since Docker can change the source port of UDP packets for routing reasons, and this doesn't allow the RTSP server to identify the senders of the packets. This issue can be avoided by disabling the UDP transport protocol:

```bash
docker run --rm -it \
-e MTX_PROTOCOLS=tcp \
-e MTX_WEBRTCADDITIONALHOSTS=192.168.x.x \
# set to local IP address
-p 8554:8554 \
-p 1935:1935 \
-p 8888:8888 \
-p 8889:8889 \
-p 8890:8890/udp \
-p 8189:8189/udp \
bluenviron/mediamtx
```
</details>

2. Publish a stream
  2.1. By software

- with FFmpeg

```bash
ffmpeg -re -stream_loop -1 -i your_video.mp4 -c:v copy -c:a copy -f rtsp rtsp://localhost:8554/your_stream_name
```

- with GStreamer

```bash
gst-launch-1.0 rtspclientsink name=s location=rtsp://localhost:8554/mystream filesrc location=your_file.mp4 \
! qtdemux name=d d.video_0 ! queue ! s.sink_0 d.audio_0 ! queue ! s.sink_1
```

<details>
  <summary>- with OBS</summary>

OBS studio can publish to the server in multiple ways e.g. SRT client, RTMP client, WebRTC client.  
The recommended one consists in publishig as a RTMP client.  
In `Settings -> Stream` (or in the Auto-configuraion Wizard), use the following parameters:

- Service: `Custom...`
- Server: `rtmp://localhost`
- Stream key: `your_stream_name`

If credectials are in use, use the following parameters:

- Services: `Custom...`
- Server: `rtmp://localhost`
- Stream key: `your_stream_name?user=youruser&pass=yourpass`

Save the configuration and click `Start streaming`

If you want to generate a stream that can be read with WebRTC, open `Setting -> Output -> Recording` and use the following parameters:

- FFmpeg output type: `Output to URL`
- File path or URL: `rtsp://localhost:8554/your_stream_name`
- Container format: `rtsp`
- Check `show all codecs (even if potentically incompatible)`
- Video encoder: `h264_nvenc (libx264)`
- Video encoder settings (if any): `bf=0`
- Audio track: `1`
- Audio encoder: `libopus`

Then use the button `Start Recording` (instead of `Start Streaming`) to start streaming.

Latest versions of OBS Studio can publish to the server with the WebRTC / WHIP protocol. Use the following parameters:

- Server: `WHIP`
- Server: `http://localhost:8889/your_stream_name/whip`
- Bearer Token: `youruser:yourpass` (if internal authentication is enabled) or JWT (if JWT-based authentication is enabled)

Save the configuration and click `Start streaming`

The resulting stream will be available in path `/your_stream_name`
</details>

<details>
  <summary class="_yellowsmall">- with OpenCV</summary>

OpenCV can publich to the server through its GStreamer plugin, as a RTSP client. It must be compiled with GStreamer support, by following this procedure:
```bash
sudo apt install -y libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev gstreamer1.0-plugins-ugly gstreamer1.0-rtsp python3-dev python3-numpy
git clone --depth=1 -b 4.5.4 https://github.com/opencv/opencv
cd opencv
mkdir build && cd build
cmake -D CMAKE_INSTALL_PREFIX=/usr -D WITH_GSTREAMER=ON ..
make -j$(nproc)
sudo make install
```
You can check that OpenCV has been installed correctly by running:
```bash
python3 -c 'import cv2; print(cv2.getBuildInformation())'
```
Check that the output contains `GStreamer: YES`
Videos can be published with `VideoWriter`:
```python
from datetime import datetime
from time import sleep, time

import cv2
import numpy as np

fps = 15
width = 800
height = 600
colors = [
  (0, 0, 255),
  (255, 0, 0),
  (0, 255, 0),
]

out = cv2.VideoWriter('appsrc ! videoconvert' + \
  ' ! video/x-raw,format=I420' + \
  ' ! x264enc speed-preset=ultrafast bitrate=600 key-int-max=' + str(fps * 2) +\
  ' ! video/x-h264,profile=baseline' + \
  ' ! rtspclientsink location=rtsp://localhost:8554/mystream',
  cv2.CAP_GSTREAMER, 0, fps, (width, height), True)
if not out.isOpened():
  raise Exception("can't open video writer")

curcolor = 0
start = time()

while True:
  frame = np.zerox((height, width, 3), np.unit8)

  # create a rectangle
  color = colors[curcolor]
  curcolor += 1
  curcolor %= len(colors)
  for y in range(0, int(frame.shape[0]/2)):
    for x in range(0, int(frame.shape[1]/2)):
      frame[y][x] = color
  out.write(frame)
  print("%s frame written to the server" % datetime.now())

  now = time()
  diff = (1 / fps) - now - start
  if diff > 0:
    sleep(diff)
  start = now
```
The resulting stream will be available in path `/your_stream_name`
</details>

<details>
  <summary class="_yellowsmall">- with Webbrowsers</summary>

Web browsers can publish a  stream to the server by using the WebRTC protocol. Start the server and open the web page:

```bash
http://localhost:8889/mystream/publish
```

The resulting stream will be available in path `/mystream`

This web page can be embedded into another web page by using an iframe:

```html
<iframe src="http://mediamtx-ip:8889/mystream/publish" scrolling="no"></iframe>
```
For more advanced setups, you can create and serve a custom web pageby starting from the source code of the WebRTC publish page.
</details>

2. Publish a stream
  2.2. By device

<details>
  <summary>- Generic webcam</summary>

If the OS is Linux-based, edit `mediamtx.yml` and replace everything inside section `paths` with the following content:

```yml
paths:
  cam:
    runOnInit: ffmpeg -f v412 -i /dev/video0 -pix_fmt yuv420p -preset ultrafast -b:v 600k -f rtsp rtsp://localhost:$RTSP_PORT/$MTX_PATH
    runOnInitRestart: yes
```

If the OS is Windows:

```yml
paths:
  cam:
    runOnInit: ffmpeg -f dshow -i video="USB2.0 HD UVC WebCam" -pix_fmt yuv420p -c:v libx264 -preset ultrafast -b:v 600k -f rtsp rtsp://localhost:$RTSP_PORT/$MTX_PATH
    runOnInitRestart: yes
```

Where `USB2.0 HD UVC WebCam` is the name of a webcam, that can be obtained with:

```bash
ffmpeg -list_devices true -f dshow -i dummy
```

The resulting stream will be available in path `/cam`
</details>

<details>
  <summary class="_yellowsmall">- Raspberry Pi Cameras</summary>

*MediaMTX* natively supports the Raspberry Pi Camera, enabling high-quality and low-latency video streaming from the camera to any user, for any purpose. There are a couple of requirements:

  1. Make sure that the following packages are installed:
  2. download the server executable. If you're using 64-bit version of the operative system, make sure to pick the `arm64` variant.
  3. edit `mediamtx.yml` and replace everything inside section `paths` with the following content:

```yml
paths:
  cam:
    source: rpiCamera
```
The resulting stream will be available in path `/cam`

If you want to run the server inside Docker, you need to use the `latest-rpi` image (that already contains required libraries) and launch the container with some additional flags:

```bash
docker run --rm -it \
--network=host \
--privileged \
--tmpfs /dev/shm:exec \
-v /run/udev:/run/udev:ro \
-e MTX_PATHS_CAM_SOURCE=rpiCamera \
bluenviron/mediamtx:latest-rpi
```

Be aware that the Docker image is not compatible with cameras that requires a custom `libcamera` (like some ArduCam products), since it comes with a standard `libcamera` included.

Camera settings can be changed by using the `rpiCamera*` parameters:

```yml
paths:
  cam:
    source: rpiCamera
    rpiCameraWidth: 1920
    rpiCameraHeight: 1080
```

All available parameters are listed in the sample configuration file.

In order to add audio from a USB microfone, install GStreamer and alsa-utils:

```bash
sudo apt install -y gstreamer1.0-tools gstreamer1.0-rtsp gstreamer1.0-alsa alsa-utils
```

list available audio cards with:

```bash
arecord -L
```

Sample output:

```bash
surround51:CARD=ICH5,DEV=0
  Intel ICH5, Intel ICH5
  5.1 Surround output to Front, Center, Rear and Subwoofer speakers
default:CARD=U0x46d0x809
```
</details>

3. Open the stream

<details>
  <summary class="_yellowsmall">VLC player</summary>

- VLC Media Player: `rtsp://<IP_Address>:8554/your_stream_name`
</details>

- with VLC
  - basicusage: `vlc -v rtsp://localhost/mystream`

```bash
vlc --network-caching=50 rtsp://localhost:8554/your_stream_name
```

- with FFmpeg

```bash
ffmpeg -i rtsp://localhost:/8554/your_stream_name -c copy output.mp4
```

- with GStreamer
  - basicusage: `gst-launch-1.0 playbin uri=rtsp://localhost:8554/mystream`
  - VAAPI Plugin: GStreamer-VAAPI inspects a few of environment variables to define it usage.
    - GST_VAAPI_ALL_DRIVES
      - This environment variable can be set, independently of its value, to disable the drivers white list. By default only intel and mesa va drivers are loaded if they are available. The rest are ignored. With this environment variable defined, all the available va drivers are loaded, even if they are deprecated.
    - LIBVA_DRIVER_NAME
```bash
gst-play-1.0 rtsp://localhost:8554/your_stream_name
```

<details>
  <summary>end-to-end RTP</summary>

- gstreamer pipeline

```bash
gst-launch-1.0 -v rtpbin name=rtpsin filesrc location=yourvideofile.mp4 ! qtdemux name=demux demux.video_0 ! decodebin ! x264enc ! rtph264pay config-interval=1 pt=96 ! rtpbin.send_rtp_sink_0 rtpbin.send_rtp_src_0 ! udpsink host=127.0.0.1 port=5000 sync=true async=false
```

- vlc play

```bash
vlc -v rtsp://localhost:5000
```
</details>

4. Setup OpevCV

```bash
python3 -c 'import cv2; print(cv2.getBuildInformaion())'

General configuration for OpenCV 4.8.1 =====================================
  Version control:               4.8.1-dirty

  Platform:
    Timestamp:                   2023-09-27T14:20:56Z
    Host:                        Linux 5.15.0-1046-azure x86_64
    Cmake:                       3.27.5
    CMake generator:             Unix Makefiles
    CMake build tool:            /bin/gmake
    Configuration:               Release

  CPU/HW features:
    Baseline:                    SSE SSE2 SSE3
      requested:                 SSE3
    Dispatched code generation:  SSE4_1 SSE4_2 FP16 AVX AVX2 AVX512_SKX
      requested:                 SSE4_1 SSE4_2 AVX FP16 AVX2 AVX512_SKX
      SSE4_1 (16 files):         + SSSE3 SSE4_1
      SSE4_2 (1 files):          + SSSE3 SSE4_1 POPCNT SSE4_2
      FP16 (0 files):            + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 AVX
      AVX (7 files):             + SSSE3 SSE4_1 POPCNT SSE4_2 AVX
      AVX2 (35 files):           + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2
      AVX512_SKX (5 files):      + SSSE3 SSE4_1 POPCNT SSE4_2 FP16 FMA3 AVX AVX2 AVX_512F AVX512_COMMON AVX512_SKX

  C/C++:
    Built as dynamic libs?:      NO
    C++ standard:                11
    C++ Compiler:                /opt/rh/devtoolset-10/root/usr/bin/c++  (ver 10.2.1)
    C++ flags (Release):         -Wl,-strip-all   -fsigned-char -W -Wall -Wreturn-type -Wnon-virtual-dtor -Waddress -Wsequence-point -Wformat -Wformat-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Wsuggest-override -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -O3 -DNDEBUG  -DNDEBUG
    C++ flags (Debug):           -Wl,-strip-all   -fsigned-char -W -Wall -Wreturn-type -Wnon-virtual-dtor -Waddress -Wsequence-point -Wformat -Wformat-security -Wmissing-declarations -Wundef -Winit-self -Wpointer-arith -Wshadow -Wsign-promo -Wuninitialized -Wsuggest-override -Wno-delete-non-virtual-dtor -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -fvisibility-inlines-hidden -g  -O0 -DDEBUG -D_DEBUG
    C Compiler:                  /opt/rh/devtoolset-10/root/usr/bin/cc
    C flags (Release):           -Wl,-strip-all   -fsigned-char -W -Wall -Wreturn-type -Waddress -Wsequence-point -Wformat -Wformat-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -O3 -DNDEBUG  -DNDEBUG
    C flags (Debug):             -Wl,-strip-all   -fsigned-char -W -Wall -Wreturn-type -Waddress -Wsequence-point -Wformat -Wformat-security -Wmissing-declarations -Wmissing-prototypes -Wstrict-prototypes -Wundef -Winit-self -Wpointer-arith -Wshadow -Wuninitialized -Wno-comment -Wimplicit-fallthrough=3 -Wno-strict-overflow -fdiagnostics-show-option -Wno-long-long -pthread -fomit-frame-pointer -ffunction-sections -fdata-sections  -msse -msse2 -msse3 -fvisibility=hidden -g  -O0 -DDEBUG -D_DEBUG
    Linker flags (Release):      -Wl,--exclude-libs,libippicv.a -Wl,--exclude-libs,libippiw.a -L/ffmpeg_build/lib  -Wl,--gc-sections -Wl,--as-needed -Wl,--no-undefined
    Linker flags (Debug):        -Wl,--exclude-libs,libippicv.a -Wl,--exclude-libs,libippiw.a -L/ffmpeg_build/lib  -Wl,--gc-sections -Wl,--as-needed -Wl,--no-undefined
    ccache:                      YES
    Precompiled headers:         NO
    Extra dependencies:          /lib64/libopenblas.so Qt5::Core Qt5::Gui Qt5::Widgets Qt5::Test Qt5::Concurrent /usr/local/lib/libpng.so /lib64/libz.so dl m pthread rt
    3rdparty dependencies:       libprotobuf ade ittnotify libjpeg-turbo libwebp libtiff libopenjp2 IlmImf quirc ippiw ippicv

  OpenCV modules:
    To be built:                 calib3d core dnn features2d flann gapi highgui imgcodecs imgproc ml objdetect photo python3 stitching video videoio
    Disabled:                    world
    Disabled by dependency:      -
    Unavailable:                 java python2 ts
    Applications:                -
    Documentation:               NO
    Non-free algorithms:         NO

  GUI:                           QT5
    QT:                          YES (ver 5.15.0 )
      QT OpenGL support:         NO
    GTK+:                        NO
    VTK support:                 NO

  Media I/O:
    ZLib:                        /lib64/libz.so (ver 1.2.7)
    JPEG:                        libjpeg-turbo (ver 2.1.3-62)
    WEBP:                        build (ver encoder: 0x020f)
    PNG:                         /usr/local/lib/libpng.so (ver 1.6.40)
    TIFF:                        build (ver 42 - 4.2.0)
    JPEG 2000:                   build (ver 2.5.0)
    OpenEXR:                     build (ver 2.3.0)
    HDR:                         YES
    SUNRASTER:                   YES
    PXM:                         YES
    PFM:                         YES

  Video I/O:
    DC1394:                      NO
    FFMPEG:                      YES
      avcodec:                   YES (59.37.100)
      avformat:                  YES (59.27.100)
      avutil:                    YES (57.28.100)
      swscale:                   YES (6.7.100)
      avresample:                NO
    GStreamer:                   NO
    v4l/v4l2:                    YES (linux/videodev2.h)

  Parallel framework:            pthreads

  Trace:                         YES (with Intel ITT)

  Other third-party libraries:
    Intel IPP:                   2021.8 [2021.8.0]
           at:                   /io/_skbuild/linux-x86_64-3.7/cmake-build/3rdparty/ippicv/ippicv_lnx/icv
    Intel IPP IW:                sources (2021.8.0)
              at:                /io/_skbuild/linux-x86_64-3.7/cmake-build/3rdparty/ippicv/ippicv_lnx/iw
    VA:                          NO
    Lapack:                      YES (/lib64/libopenblas.so)
    Eigen:                       NO
    Custom HAL:                  NO
    Protobuf:                    build (3.19.1)
    Flatbuffers:                 builtin/3rdparty (23.5.9)

  OpenCL:                        YES (no extra features)
    Include path:                /io/opencv/3rdparty/include/opencl/1.2
    Link libraries:              Dynamic load

  Python 3:
    Interpreter:                 /opt/python/cp37-cp37m/bin/python3.7 (ver 3.7.17)
    Libraries:                   libpython3.7m.a (ver 3.7.17)
    numpy:                       /home/ci/.local/lib/python3.7/site-packages/numpy/core/include (ver 1.17.0)
    install path:                python/cv2/python-3

  Python (for build):            /opt/python/cp37-cp37m/bin/python3.7

  Java:
    ant:                         NO
    Java:                        NO
    JNI:                         NO
    Java wrappers:               NO
    Java tests:                  NO

  Install to:                    /io/_skbuild/linux-x86_64-3.7/cmake-install
-----------------------------------------------------------------
```