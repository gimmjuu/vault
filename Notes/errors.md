---
tags:
  - DEVELOP
---

# Errors
## 🎇 Image file
### `libpng warning: iCCP: known incorrect sRGB profile`
- 이미지 파일 저장 옵션 문제
```bash
sudo apt-get update
sudo apt-get install imagemagick
cp -R <source_folder> <destination_folder>  # 이미지 백업
mogrify *
```
# Clang Error
## 🎇 C

## 🎇 C & C++
### `Error: command 'gcc' failed: No such file or directory: 'gcc'`
- GCC 컴파일러 미설치 문제
```bash
apt-get update && apt-get -y install gcc
```

## 🎇 C++
### `fatel error: wincodec.h 파일을 찾을 수 없습니다.`
- Visual Studio에 포함된 헤더파일(`wincodec.h`)
```
(수행) Build C++ in Visual Studio. (Not Visual Studio Code.)
```

### `fatal error RC1015: cannot open include file 'afxres.h'`
- MFC에 포함된 헤더파일(`afxres.h`)
```
(수행) Tools > Get tools and features > ... MFC ... install
```


# Linux Error
## 🎇 Linux & Python
### `ERROR: Could not install packages due to an OSError: [WinError 5]`
- 사용자 계정 권한 문제
```bash
pip install {package} --user
```

## 🎇 Linux & torch
### `torch install killed`
- Linux Ubuntu에서 torch 설치할 때, 메모리 문제
```bash
pip install torch --no-cache-dir
```

# Docker Error
## 🎇 Docker
### `VS Code Error: connect EACCES /var/run/docker.sock`
- vscode에서 docker를 사용할 때, 사용자 계정 권한 문제
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
```

## 🎇 Docker & OpenCV
### `ImportError: libGL.so.1: cannot open shared object file: No such file or directory`
- Docker 환경에서 OpenCV를 사용할 때, Docker에 누락된 cv2 종속성 문제
```bash
apt-get update && apt-get install libgl1
```
또는
```bash
apt-get update && apt-get install ffmpeg libsm6 libxext6 -y
```

# Python Error
## 🎇 Python & OpenCV
### `cv2.error: OpenCV(4.8.1) ~\window.cpp:1268: error:(-2:Unspecified error) The function is not implemented. Rebuild the library with Windows, GTK+ 2.x or Cocoa support. If you are on Ubuntu or ...`
- OpenCV 이전버전 오류로 예상
```bash
pip uninstall opencv-python
pip install opencv-python
```

### `cv2.error: (-5:Bad argument) in function 'circle' ... Can't parse 'center'. Sequence item with index 0 has a wrong type`
- opencv 버전 업데이트로 도형 관련 메소드에는 정수 인수만 입력 가능
```
(수행) Replace params with int(params)
```

### `[WARN:1@21.402] global loadsave.cpp:248 cv::findDecoder imread_('C:\Users\문서\...'): can't open/read file: check file path/integrity`
- opencv 한글 경로 인식 불가 문제
```
(수행) Replace " image = cv2.imread(image) " to
" image = np.fromfile(image, np.uint8)
  image = cv2.imdecode(image, cv2.IMREAD_COLOR) "
```

### `cv2.error: OpenCV(4.8.1) D:\a\opencv-python\opencv-python\opencv\modules\imgcodecs\src\loadsave.cpp:1120: error: (-2:Unspecified error) could not find encoder for the specified extension in function 'cv::imencode'`
- imencode(확장자, 이미지) 메소드에서 확장자 지정 시 < . > 을 누락해서 발생하는 오류
```
(수행) Replace 'cv2.imencode("jpg", image)' to 'cv2.imencode(".jpg", image)'
```

## 🎇 Python & torch
### `RuntimeError: DataLoader worker (pid 576) is killed by signal: Bus error.`
```python
 DataLoader(..., num_workers=0)
```
또는 Swap 메모리 늘리기
```bash
sudo swapoff -v /swapfile  # 기존 Swap 비활성화
sudo fallocate -l 4G /swapfile # Swap으로 사용할 메모리 할당
sudo chmod 600 /swapfile  # 해당 메모리 사용 권한 수정
sudo mkswap /swapfile  # 해당 메모리를 Swap으로 설정
sudo swapon /swapfile  # Swap 활성화
```

### `RuntimeError: view size is not compatible with input tensor's size and stride (at least one dimension spans across two contiguous subspaces). Use .reshape(...) instead.`
- torch 버전 업그레이드로 인한 메소드 변경
```
(수행) Replace the "view()" function with "reshape()" function
```

### `UserWarning: masked_scatter_ received a mask with dtype torch.uint8, this behavior is now deprecated,please use a mask with dtype torch.bool instead.`
- torch 버전 업그레이드로 인한 메소드 변경
```
(수행) Replace the ".byte()" method with ".bool()" method
```

### `UserWarning: The epoch parameter in 'scheduler.step()' was not necessary and is being deprecated where possible. Please use 'scheduler.step()' to step the scheduler.`
- 미해결...

## 🎇 Python & torchvision
### `ImportError: cannot import name 'model_urls' from 'torchvision.models.vgg'`
- torchvision 버전 업그레이드로 인한 경로 삭제
```
(수행) Delete code lines,
① from torchvision.models.vgg import model_urls
② model_urls['vgg16_bn'] = model_urls['vgg16_bn'].replace('https://', 'http://')
```

## 🎇 Python & tensorflow
### `AttributeError: module 'tensorflow' has no attribute 'contrib'`
- tf1 vs tf2 버전 문제
```
(수행) Replace "tf.contrib" with "tf.compat.v1.estimator"
```

### `ModuleNotFoundError: No module named 'tensorflow.examples'`
- tf1 vs tf2 버전 문제
```
(수행) Replace "input_data.read_data_sets("./MNIST_data/")" with "tf.keras.datasets.mnist"
```

### `AttributeError: module 'tensorflow' has no attribute 'logging'.`
- tf1 vs tf2 버전 문제
```
(수행) Replace "tf.logging.set_verbosity(tf.logging.ERROR)" with "tf.compat.v1.logging.set_verbosity(tf.compat.v1.logging.ERROR)"
```

### `RuntimeError: tf.placeholder() is not compatible with eager execution.`
- tf1 vs tf2 버전 문제
```python
tf.compat.v1.disable_eager_execution()
```
또는
```python
from tensorflow import compat.v1 as tf1

tf1.disable_eager_execution()
```

## 🎇 Python & cython
### `Building wheel for mmpycocotools (setup.py) ... error`
- cython 종속성 누락 문제
```bash
pip3 uninstall cython && pip3 install cython==0.29.33
```

## 🎇 Python & PIL
### `AttributeError: module 'PIL.Image' has no attribute 'ANTIALIAS'`
- Pillow 10 이후 삭제된 속성
```
(수행) Replace "Image.ANTIALIAS" with "Image.Resampling.LANCZOS"
```

## 🎇 Python & Matplotlib
### `UserWarning: FigureCanvasAgg is non-interactive, and thus cannot be shown plt.show()`
- 의존성 패키지 문제
```bash
pip install PyQt5
```

## 🎇 Python & Numpy
### `RuntimeError: Numpy is not available`
- numpy가 설치되어 있지 않거나 버전 의존성 문제
```bash
pip install numpy --upgrade
```

### `AttributeError: module 'numpy' has no attribute 'object'.`
- numpy 1.24.* 버전 문제
```bash
pip uninstall numpy && pip install numpy==1.23.5
```

### `Original error was: No module named 'numpy.core._multiarray_umath'`
- numpy 이전 버전 문제
```bash
pip install numpy --upgrage
```

## 🎇 Python & XML
### `AttributeError: 'xml.etree.ElementTree.Element' object has no attribute 'getiterator'`
- python 내장 라이브러리 xml에서 삭제된 메소드
```
(수행) Replace `getiterator` with `iter`
```

## 🎇 Pydub
### `FileNotFoundError: [WinError 2] 지정된 파일을 찾을 수 없습니다`
- 코덱 문제
```
(수행) ffmpeg 환경변수 등록
```

## 🎇 PyInstaller
### `win32ctypes.pywin32.pywintypes.error: (225, 'BeginUpdateResourceW', '파일에 바이러스 또는 기타 사용자 동의 없이 설치된 소프트웨어가 있기 때문에 작업이 완료되지 않았습니다.')`
- Windows OS 보안 문제
```bash
pip install pyinstaller==5.13.2
```

### `OSError: [Errno 22] Invalid argument`
- Windows Defender 문제
```bash
pip install pyinstaller==5.6.2
```
또는
```
(수행) 시작 > 설정 > 업데이트 및 보안 > Windows 보안 > 바이러스 및 위협 방지 > 설정 관리: 원하는 스크립트가 포함된 폴더를 예외로 추가
```

### `AttributeError("module '{module name}' has no attribute 'open'")`
- hook 파일이 없는 라이브러리를 import한 경우
```bash
pyinstaller -F -w --collect-data {module name} -n {appname} {run.py}
```