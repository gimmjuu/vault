---
tags:
  - TAG ONLY CAP
---

# OpenCV
## 🎇 OpenCV 설치와 기초 사용

```
$ pip install opencv-python
```

```Python
import sys
import cv2

# opencv-python 버전 확인
print("Hello OpenCV", cv2.__version__)

# 이미지 경로
image_path = r"./assets/lenna.bmp"
# 이미지 불러오기
image = cv2.imread(image_path)
# 이미지 로드 실패 시 None을 반환
if image is None:
    print("Image load failed!")
    sys.exit()
# 이미지 출력을 위한 윈도우 이름 저장
cv2.namedWindow("Image")
# 이미지를 윈도우에 할당하여 출력
cv2.imshow("Image", image)
# 키 입력 대기(블로킹)
cv2.waitKey()
# 키 입력 시 OpenCV에서 할당한 모든 윈도우 종료
cv2.destroyAllWindows()
```

## 🎇 Mat (Default Image Container)
### 📌 디지털 이미지
- 무수히 많은 픽셀들이 모여서 만드는 형상
- 0 <= 이미지 픽셀 값 <= 255
- IplImage 시절에는 메모리 할당 후 수동으로 해제해야 했음
- Mat 사용 시 클래스 생성자에서 메모리를 할당하고 클래스 소멸자에서 메모리를 해제

### 📌 Mat 클래스
- Matrix header & Data matrix pointer
    1. Matrix header (매트릭스 헤더)
        - 매트릭스 크기, 저장 방법, 주소 정보 등
        - 헤더 크기는 일정
    2. Data matrix pointer (데이터 매트릭스 포인터)
        - 실제 이미지 픽셀값들을 저장
        - 이미지 채널에 따라 매트릭스 차원이 달라짐
            - Grayscale은 1차원, RGB는 3차원
        - 이미지에 따라 크기가 달라짐

### 📌 참조 카운팅 Reference Counting System
- 영상 처리 속도 개선
- 각각의 Mat 객체들은 자신만의 매트릭스 헤더를 가지면서 같은 데이터 매트릭스를 공유 자원으로 사용하는 방식
- Mat 객체 간에 매트릭스 헤더를 복사하고 매트릭스 포인터만 실제 데이터를 가리키도록 하여 이미지 데이터 복사 속도를 개선
- 데이터 매트릭스를 사용하는 Mat 객제가 없을 때 메모리에서 해제함

- 서로 같은 데이터 매트릭스를 공유하는 Mat 객체들의 경우 하나의 객체를 이진화하면 다른 객체도 이진화된 결과를 볼 수 있음
- Mat 객체가 데이터 매트릭스의 일부만 가리키도록 할 수 있음
    - Rect
        - 사각형 영역을 가져옴
    - Range
        - 특정 열과 행 범위만 가져옴
- 부분만 가리키는 객체를 이진화 해도 원본 이미지에 영향을 줌
    - 결국 참조이기 때문

### 📌 복사본 생성 clone, CopyTo
- 데이터 매트릭스를 복사해서 새로운 데이터 매트릭스로 할당함
    - 깊 은 복 사

### 📌 Mat 객체 생성
1. Mat() 생성자
2. 배열과 생성자
```CPP
    unsigned char t_arr[2][2] = {{1, 2}, {3, 4}};
    Mat t_mat(2, 2, CV_8UC1, &t_arr);
    cout << t_mat << endl;
```
`result`    [  1,  2;
                3,  4]
3. create() 함수
```CPP
Mat mat;
mat.create(2, 2, CV_8UC1);
```

4. like MARLAB
```CPP
Mat mat1 = Mat::eye(2, 2, CV_8UC1);
Mat mat2 = Mat::ones(2, 2, CV_8UC1);
Mat mat3 = Mat::zeros(2, 2, CV_8UC1);
```

5. ','로 나열된 값으로 초기화
```CPP
Mat mat = (Mat <double>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
```

6. Mat 객체 복사 & 새로운 객체 생성
```CPP
Mat rowClone = mat.row(1).clone();
Mat columnClone = mat.colRange(0, 2).clone();
```

### 📌 예제: Mat
```Python
import numpy as np
import cv2

def func1():
    '''이미지 매트릭스 채널'''
    img = cv2.imread("./assets/cat.bmp", cv2.IMREAD_GRAYSCALE)
    if img is None:
        print("Image load failed!")
        return
    print("type(IMG-1):", type(img))
    # type(IMG-1): <class 'numpy.ndarray'>
    print("IMG-1.shape:", img.shape)
    # IMG-1.shape: (480, 640)
    if len(img.shape) == 2:
        print("IMG-1 is a grayscale image")
    elif len(img.shape) == 3:
        print("IMG-1 is a truecolor image")
    # IMG-1 is a grayscale image
    cv2.imshow("IMG-1", img)
    cv2.waitKey()
    cv2.destroyAllWindows()

def func2():
    '''이미지 매트릭스'''
    img1 = np.empty((480, 640), np.uint8)       # grayscale image
    img2 = np.zeros((480, 640, 3), np.uint8)    # color image
    img3 = np.ones((480, 640), np.uint32)       # 1's matrix
    img4 = np.full((480, 640), 0, np.float32)   # Fill with 0.0
    mat = np.array([[11, 12, 13, 14],
                    [21, 22, 23, 24],
                    [31, 32, 33, 34]]).astype(np.uint8)
    mat[0, 1] = 100 # element at x=1, y=0
    mat[2, :] = 200
    print(mat)
    # [[ 11 100  13  14]
    # [ 21  22  23  24]
    # [200 200 200 200]]

def func3():
    '''참조와 복사'''
    img1 = cv2.imread("./assets/cat.bmp")
    img2 = img1
    img3 = img1.copy()
    YELLOW = (0, 255, 255)
    img1[:, :] = YELLOW # yellow
    cv2.imshow('img1', img1)
    cv2.imshow('img2', img2)
    cv2.imshow('img3', img3)
    cv2.waitKey()
    cv2.destroyAllWindows()

def func4():
    '''부분 참조와 부분 복사'''
    img1 = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
    img2 = img1[200:400, 200:400]
    img3 = img1[200:400, 200:400].copy()
    img2 += 20
    cv2.imshow('img1', img1)
    cv2.imshow('img2', img2)
    cv2.imshow('img3', img3)
    cv2.waitKey()
    cv2.destroyAllWindows()

def func5():
    '''요소 연산'''
    mat1 = np.array(np.arange(12)).reshape(3, 4)
    print("mat1:\n", mat1)
    # mat1:
    # [[ 0  1  2  3]
    # [ 4  5  6  7]
    # [ 8  9 10 11]]
    h, w = mat1.shape[:2]
    mat2 = np.zeros(mat1.shape, type(mat1))
    for j in range(h):
        for i in range(w):
            mat2[j, i] = mat1[j, i] + 10
    print("mat2:\n", mat2)
    # mat2:
    # [[10 11 12 13]
    # [14 15 16 17]
    # [18 19 20 21]]

def func6():
    '''매트릭스 연산'''
    mat1 = np.ones((3, 4), np.int32)    # 1's matrix
    mat2 = np.arange(12).reshape(3, 4)
    mat3 = mat1 + mat2
    mat4 = mat2 * 2
    print("mat1:", mat1, sep='\n')
    # mat1:
    # [[1 1 1 1]
    # [1 1 1 1]
    # [1 1 1 1]]
    print("mat2:", mat2, sep='\n')
    # mat2:
    # [[ 0  1  2  3]
    # [ 4  5  6  7]
    # [ 8  9 10 11]]
    print("mat3:", mat3, sep='\n')
    # mat3:
    # [[ 1  2  3  4]
    # [ 5  6  7  8]
    # [ 9 10 11 12]]
    print("mat4:", mat4, sep='\n')
    # mat4:
    # [[ 0  2  4  6]
    # [ 8 10 12 14]
    # [16 18 20 22]]
```

## 🎇 Mask operations
- 마스크 배열(=커널)을 사용해 이미지 상 픽셀값을 다시 계산
- 현재 위치의 픽셀 포함 이웃 픽셀값에 가중치를 곱해 현재 픽셀값을 결정
- 에지 검출, 잡음 제거 등 여러가지 효과를 냄

### 📌 예 1: 선명하게 마스크
```CPP
void Sharpen(const Mat& Origin, Mat& Result){
    CV_Assert(Origin.depth() == CV_8U);
    // accept only uchar images
    Result.create(Origin.size(), Origin.type());
    const int nChannels = Origin.channels();

    for(int y = 1; y < (Origin.rows - 1); ++y){
        const uchar* previous = Origin.ptr<uchar>(y - 1);
        const uchar* current  = Origin.ptr<uchar>(y    );
        const uchar* next     = Origin.ptr<uchar>(y + 1);

        uchar* output = Result.ptr<uchar>(y);

        for(int x = nChannels; x < nChannels * (Origin.cols - 1); ++x){
            *output++ = saturate_cast<uchar>(
                5 * current[x] - current[x-nChannels] - current[x+nChannels] - previous[x] - next[x]
            );
        }
    }

    Result.row(0).setTo(Scalar(0));
    Result.row(Result.rows - 1).setTo(Scalar(0));
    Result.col(0).setTo(Scalar(0));
    Result.col(Result.cols - 1).setTo(Scalar(0));
}
```

### 📌 예 2: filter2D 함수를 활용한 선명하게
```CPP
Mat sharpen_kernel = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
filter2D(origin_image, sharpen_image, origin_image.depth(), sharpen_kernel);
```


## 🎇 Image Blending
➰　두 이미지 섞기(합치기)

    𝑔(𝜒) = (1-𝛼)f₀(𝜒)+𝛼f₁(𝜒)

- 선형 blend 연산자는 𝛼를 0에서 1로 변화시키면서 두 개의 이미지(또는 비디오) 간에 cross disolve를 수행
- 두 장의 이미지는 𝛼값에 따라 다르게 겹쳐짐 (𝛼 = alpha 값)
```Python
addWeighted(image1, alpha, image2, (1.0-alpha), 0.0, result);
```

## 🎇 Hough Line
➰　직선 검출
- HoughLines() 및 HoughLinesP()
    - 이미지에서 직선을 감지
    - Transform을 위한 Edge 검출 전처리 필요

### 📌 예제: HoughLines
```Python
import cv2
import numpy as np

origin_image = cv2.imread(image_path)

gray = cv2.cvtColor(origin_image, cv2.COLOR_BGR2GRAY)
# 외곽선 검출
edged = cv2.Canny(gray, 50, 200)
recolored = cv2.cvtColor(edged, cv2.COLOR_GRAY2BGR)
# 직선 성분 검출
lines = cv2.HoughLinesP(edged, 1, np.pi / 180., 160, minLineLength=50, maxLineGap=5)

if lines is not None:
    for i, _ in enumerate(lines.shape[0]):
        pt1 = (lines[i][0][0], lines[i][0][1])  # 시작점 좌표
        pt2 = (lines[i][0][2], lines[i][0][3])  # 끝점 좌표
        cv2.Line(recolored, pt1, pt2, (0, 0, 255), 2, cv2.LINE_AA)

cv2.imshow("DST", recolored)
cv2.waitkey()
cv2.destroyAllWindows()
```

## 🎇 Basic Operations
### 📌 예제: drawing
```Python
import numpy as np
import cv2

def drawLines():
    '''선분 그리기'''
    img = np.full((400, 400, 3), 255, np.uint8) # 400 * 400 size, 3 channel, White Background

    cv2.line(img, (50, 50), (200, 50), RED)
    cv2.line(img, (50, 100), (200, 100), PINK, 3)
    cv2.line(img, (50, 150), (200, 150), BLUE, 10)

    cv2.line(img, (250, 50), (350, 100), RED, 1, cv2.LINE_4)
    cv2.line(img, (250, 70), (350, 120), PINK, 1, cv2.LINE_8)
    cv2.line(img, (250, 90), (350, 140), BLUE, 1, cv2.LINE_AA)

    cv2.arrowedLine(img, (50, 200), (150, 200), RED, 1)
    cv2.arrowedLine(img, (50, 250), (350, 250), PINK, 1)
    cv2.arrowedLine(img, (50, 300), (350, 300), BLUE, 1, cv2.LINE_8, 0, 0.05)

    cv2.drawMarker(img, (50, 350), RED, cv2.MARKER_CROSS)
    cv2.drawMarker(img, (100, 350), RED, cv2.MARKER_TILTED_CROSS)
    cv2.drawMarker(img, (150, 350), RED, cv2.MARKER_STAR)
    cv2.drawMarker(img, (200, 350), RED, cv2.MARKER_DIAMOND)
    cv2.drawMarker(img, (250, 350), RED, cv2.MARKER_SQUARE)
    cv2.drawMarker(img, (300, 350), RED, cv2.MARKER_TRIANGLE_UP)
    cv2.drawMarker(img, (350, 350), RED, cv2.MARKER_TRIANGLE_DOWN)

    cv2.imshow("IMG", img)
    cv2.waitKey()
    cv2.destroyAllWindows()

def drawPolys():
    '''다각형 그리기'''
    img = np.full((400, 400, 3), 255, np.uint8)

    cv2.rectangle(img, (50, 50), (150, 100), RED, 2)
    cv2.rectangle(img, (50, 150), (150, 200), BROWN, -1)

    cv2.circle(img, (300, 120), 30, MINT, -1, cv2.LINE_AA)
    cv2.circle(img, (300, 120), 60, BLUE, 3, cv2.LINE_AA)

    cv2.ellipse(img, (120, 300), (60, 30), 20, 0, 270, MINT, cv2.FILLED, cv2.LINE_AA)
    cv2.ellipse(img, (120, 300), (100, 50), 20, 0, 360, GREEN, 2, cv2.LINE_AA)

    pts = np.array([[250, 250], [300, 250], [300, 300], [350, 300], [350, 350], [250, 350]])
    cv2.polylines(img, [pts], True, PINK, 2)

    cv2.imshow("IMG", img)
    cv2.waitKey()
    cv2.destroyAllWindows()

def drawText1():
    img = np.full((500, 800, 3), 255, np.uint8)

    cv2.putText(img, "FONT_HERSHEY_SIMPLEX", (20, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, RED)
    cv2.putText(img, "FONT_HERSHEY_PLAIN", (20, 100), cv2.FONT_HERSHEY_PLAIN, 1, RED)
    cv2.putText(img, "FONT_HERSHEY_DUPLEX", (20, 150), cv2.FONT_HERSHEY_DUPLEX, 1, RED)
    cv2.putText(img, "FONT_HERSHEY_COMPLEX", (20, 200), cv2.FONT_HERSHEY_COMPLEX, 1, BLUE)
    cv2.putText(img, "FONT_HERSHEY_TRIPLEX", (20, 250), cv2.FONT_HERSHEY_TRIPLEX, 1, BLUE)
    cv2.putText(img, "FONT_HERSHEY_COMPLEX_SMALL", (20, 300), cv2.FONT_HERSHEY_COMPLEX_SMALL, 1, BLUE)
    cv2.putText(img, "FONT_HERSHEY_SCRIPT_SIMPLEX", (20, 350), cv2.FONT_HERSHEY_SCRIPT_SIMPLEX, 1, PINK)
    cv2.putText(img, "FONT_HERSHEY_SCRIPT_COMPLEX", (20, 400), cv2.FONT_HERSHEY_SCRIPT_COMPLEX, 1, PINK)
    cv2.putText(img, "FONT_HERSHEY_COMPLEX | FONT_ITALIC", (20, 450), cv2.FONT_HERSHEY_COMPLEX | cv2.FONT_ITALIC, 1, BLUE)

    cv2.imshow("IMG", img)
    cv2.waitKey()
    cv2.destroyAllWindows()

def drawText2():
    img = np.full((200, 640, 3), 255, np.uint8)

    text = "Hello, OpenCV"
    fontFace = cv2.FONT_HERSHEY_TRIPLEX
    fontScale = 2.0
    thickness = 1

    sizeText, _ = cv2.getTextSize(text, fontFace, fontScale, thickness)

    org = ((img.shape[1] - sizeText[0]) // 2, (img.shape[0] + sizeText[1]) // 2)
    cv2.putText(img, text, org, fontFace, fontScale, BLUE, thickness)
    cv2.rectangle(img, org, (org[0] + sizeText[0], org[1] - sizeText[1]), GREEN, 1)

    cv2.imshow("IMG", img)
    cv2.waitKey()
    cv2.destroyAllWindows()
```

### 📌 예제: keyboard()
```Python
import sys
import numpy as np
import cv2

img = cv2.imread("./assets/lenna.bmp")
if img is None:
    print("Image load failed!")
    # sys.exit()
    return
cv2.namedWindow("IMG")
cv2.imshow("IMG", img)
while True:
    keycode = cv2.waitKey()
    if keycode == ord("i") or keycode == ord("I"):
        img = ~img  # 색상 반전
        cv2.imshow("IMG", img)
    elif keycode == 27 or keycode == ord("q") or keycode == ord("Q"):
        break
cv2.destroyAllWindows()
```

### 📌 예제: mouse()
```Python
import sys
import numpy as np
import cv2

img = cv2.imread("./assets/lenna.bmp")
if img is None:
    print("Image load failed!")
    return

def on_mouse(event, x, y, flags, param):
    global oldx, oldy
    if event == cv2.EVENT_LBUTTONDOWN:
        oldx, oldy = x, y
        print(f"EVENT_LBUTTONDOWN: {x}, {y}")
    elif event == cv2.EVENT_LBUTTONUP:
        print(f"EVENT_LBUTTONUP: {x}, {y}")
    elif event == cv2.EVENT_MOUSEMOVE:
        if flags & cv2.EVENT_FLAG_LBUTTON:
            cv2.line(img, (oldx, oldy), (x, y), YELLOW, 2)
            cv2.imshow("IMG", img)
            oldx, oldy = x, y

cv2.namedWindow("IMG")
cv2.setMouseCallback("IMG", on_mouse)

cv2.imshow("IMG", img)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: storage
```Python
import os
import numpy as np
import cv2

filedir = "./saves"
if not os.path.exists(filedir):
    os.mkdir(filedir)
filename = "mydata.json"
# filename = "mydata.xml"
# filename = "mydata.yml"

def writeData():
    name = "Jane"
    age = 10
    pt1 = (100, 200)
    scores = (80, 90, 50)
    mat = np.array([[1.0, 1.5], [2.0, 3.2]], dtype=np.float32)

    fs = cv2.FileStorage(rf"{filedir}/{filename}", cv2.FILE_STORAGE_WRITE)
    if not fs.isOpened():
        print("File open failed!")
        return
    fs.write("name", name)
    fs.write("age", age)
    fs.write("point", pt1)
    fs.write("scores", scores)
    fs.write("data", mat)
    fs.release()

def readData():
    fs = cv2.FileStorage(rf"{filedir}/{filename}", cv2.FILE_STORAGE_READ)
    if not fs.isOpened():
        print("File open failed!")
        return
    name = fs.getNode("name").string()
    age = int(fs.getNode("age").real())
    pt1 = tuple(fs.getNode("point").mat().astype(np.int32).flatten())
    scores = tuple(fs.getNode("scores").mat().flatten())
    mat = fs.getNode("data").mat()
    fs.release()

    print("name:", name)
    print("age:", age)
    print("point:", pt1)
    print("scores:", scores)
    print("data:\n", mat)
```

### 📌 예제: trackbar
```Python
import numpy as np
import cv2

img = np.zeros((400, 400), np.uint8)

def saturated(value):
    if value > 255:
        value = 255
    elif value < 0:
        value = 0
    return value

def on_level_change(pos):
    img[:] = saturated(pos * 16)
    cv2.imshow("Image", img)

cv2.namedWindow("image")
cv2.createTrackbar("level", "image", 0, 16, on_level_change)

cv2.imshow("image", img)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: utils
```Python
import numpy as np
import cv2

def mask_setTo():
    src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_COLOR)
    mask = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
    if src is None or mask is None:
        print("Image load failed!")
        return
    src[mask > 0] = YELLOW
    # src[mask>128] = YELLOW
    cv2.imshow("src", src)
    cv2.imshow("mask", mask)
    cv2.waitKey()
    cv2.destroyAllWindows()

def mask_copyTo():
    src = cv2.imread("./assets/airplane.bmp", cv2.IMREAD_COLOR)
    mask = cv2.imread("./assets/mask_plane.bmp", cv2.IMREAD_GRAYSCALE)
    dst = cv2.imread("./assets/field.bmp", cv2.IMREAD_COLOR)
    if src is None or mask is None or dst is None:
        print("Image load failed!")
        return
    cv2.copyTo(src, mask, dst)
    dst[mask > 0] = src[mask > 0]
    cv2.imshow("src", src)
    cv2.imshow("dst", dst)
    cv2.imshow("mask", mask)
    cv2.waitKey()
    cv2.destroyAllWindows()

def time_inverse():
    src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
    if src is None:
        print("Image load failed!")
        return
    dst = np.empty(src.shape, dtype=src.dtype)
    tm = cv2.TickMeter()
    tm.start()
    for y in range(src.shape[0]):
        for x in range(src.shape[1]):
            dst[y, x] = 255 - src[y, x]
    tm.stop()
    print(f"Image inverse implementation took {tm.getTimeMilli():4.3f} ms")
    # Image inverse implementation took 397.459 ms
    cv2.imshow("src", src)
    cv2.imshow("dst", dst)
    cv2.waitKey()
    cv2.destroyAllWindows()

def useful_func():
    img = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
    if img is None:
        print("Image load failed!")
        return
    sum_img = np.sum(img)
    mean_img = np.mean(img, dtype=np.int32)
    print("Sum:", sum_img)
    # Sum: 32518590
    print("Mean:", mean_img)
    # Mean: 124
    minVal, maxVal, minPos, maxPos = cv2.minMaxLoc(img)
    print("minVal is", minVal, "at", minPos)
    # minVal is 25.0 at (508, 71)
    print("maxVal is", maxVal, "at", maxPos)
    # maxVal is 245.0 at (116, 273)
    src = np.array([[-1, -0.5, 0, 0.5, 1]], dtype=np.float32)
    dst = cv2.normalize(
        src, None, 0, 255, cv2.NORM_MINMAX, cv2.CV_8U
    )  # 0-255 정규화
    print("src:", src)
    # src: [[-1.  -0.5  0.   0.5  1. ]]
    print("dst:", dst)
    # dst: [[  0  64 128 191 255]]
    print("round(2.5) is", round(2.5))
    # round(2.5) is 2
    print("round(2.51) is", round(2.51))
    # round(2.51) is 3
    print("round(3.499) is", round(3.499))
    # round(3.499) is 3
    print("round(3.5) is", round(3.5))
    # round(3.5) is 4
```

### 📌 예제: video
```Python
import numpy as np
import cv2

def camera_in():
    cap = cv2.VideoCapture(0)
    if not cap.isOpened():
        print("Camera open failed!")
        return
    print("Frame width:", int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)))
    print("Frame height:", int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)))
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        inversed = ~frame
        cv2.imshow("Frame", frame)
        cv2.imshow("Inversed", inversed)
        if cv2.waitKey(10) == 27:
            break
    cv2.destroyAllWindows()

def video_in():
    cap = cv2.VideoCapture("./assets/stopwatch.avi")
    if not cap.isOpened():
        print("Video open failed!")
        return
    print("Frame width:", int(cap.get(cv2.CAP_PROP_FRAME_WIDTH)))
    # Frame width: 640
    print("Frame height:", int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT)))
    # Frame height: 480
    print("Frame count:", int(cap.get(cv2.CAP_PROP_FRAME_COUNT)))
    # Frame count: 378
    fps = cap.get(cv2.CAP_PROP_FPS)
    print("FPS:", fps)
    # FPS: 30.0
    delay = round(1000 / fps)
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        inversed = ~frame
        cv2.imshow("Frame", frame)
        cv2.imshow("Inversed", inversed)
        if cv2.waitKey(10) == 27:
            break
    cv2.destroyAllWindows()

def camera_in_video_out():
    cap = cv2.VideoCapture(0)
    if not cap.isOpened():
        print("Camera open failed!")
        return
    w = round(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    h = round(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
    fps = cap.get(cv2.CAP_PROP_FPS)
    fourcc = cv2.VideoWriter_fourcc(*"DIVX")  # *"DIVX" == 'D','I','V','X'
    delay = round(1000 / fps)
    outputVideo = cv2.VideoWriter("./assets/output.avi", fourcc, fps, (w, h))
    if not outputVideo.isOpened():
        print("File open failed!")
        return
    while True:
        ret, frame = cap.read()
        if not ret:
            break
        inversed = ~frame
        outputVideo.write(inversed)
        cv2.imshow("Frame", frame)
        cv2.imshow("Inversed", inversed)
        if cv2.waitKey(delay) == 27:
            break
    cv2.destroyAllWindows()
```

## 🎇 Brightness 밝기 조절
### 📌 예제: Brightness1
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dst = cv2.add(src, 100)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: Brightness2
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dst = np.empty(src.shape, src.dtype)
for y in range(src.shape[0]):
    for x in range(src.shape[1]):
        dst[y, x] = src[y, x] + 100
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: Brightness3
```Python
import numpy as np
import cv2
    
def saturated(value):
    if value > 255:
        value = 255
    elif value < 0:
        value = 0
    return value

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dst = np.empty(src.shape, dtype=src.dtype)
for y in range(src.shape[0]):
    for x in range(src.shape[1]):
        dst[y, x] = saturated(src[y, x] + 100)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: Brightness4
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"

def update(pos):
    dst = cv2.add(src, pos)
    cv2.imshow("dst", dst)
    
cv2.namedWindow("dst")
cv2.createTrackbar("Brightness", "dst", 0, 100, update)
update(0)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Contrast 명암비 조절
### 📌 예제: Contrast1
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
s = 2.0
dst = cv2.multiply(src, s)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: Contrast2
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
# dst(x,y) = src(x,y) + (src(x,y) - 128) * alpha
alpha = 1.0
dst = np.clip(src + (src - 128.) * alpha, 0, 255).astype(np.uint8)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Histogram 히스토그램
### 📌 예제: utils
```Python
import numpy as np
import cv2

def calcGrayHist(img):
    channels = [0]
    histSize = [256]
    histRange = [0, 256]
    hist = cv2.calcHist([img], channels, None, histSize, histRange)
    return hist

def getGrayHistImage(hist):
    histMax = np.max(hist)
    imgHist = np.full((100, 256), 255, dtype=np.uint8)
    for x in range(256):
        pt1 = (x, 100)
        pt2 = (x, 100 - int(hist[x, 0] * 100 / histMax))
        cv2.line(imgHist, pt1, pt2, 0)
    return imgHist
```

### 📌 예제: histogram_stretching()
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/hawkes.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
gmin = float(np.min(src))
gmax = float(np.max(src))
dst = ((src - gmin) * 255.0 / (gmax - gmin)).astype(np.uint8)
cv2.imshow("src", src)
cv2.imshow("srcHist", getGrayHistImage(calcGrayHist(src)))
cv2.imshow("dst", dst)
cv2.imshow("dstHist", getGrayHistImage(calcGrayHist(dst)))
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: histogram_equalization()
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/hawkes.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dst = cv2.equalizeHist(src)
cv2.imshow("src", src)
cv2.imshow("srcHist", getGrayHistImage(calcGrayHist(src)))
cv2.imshow("dst", dst)
cv2.imshow("dstHist", getGrayHistImage(calcGrayHist(dst)))
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Arithmetic 산술 연산
```Python
import sys
import numpy as np
import cv2
from matplotlib import pyplot as plt

src1 = cv2.imread("./assets/lenna256.bmp", cv2.IMREAD_GRAYSCALE)
src2 = cv2.imread("./assets/square.bmp", cv2.IMREAD_GRAYSCALE)
assert src1 is not None and src2 is not None, "Image load failed!"
dst1 = cv2.add(src1, src2)
dst2 = cv2.addWeighted(src1, 0.5, src2, 0.5, 0.0)
dst3 = cv2.subtract(src1, src2)
dst4 = cv2.absdiff(src1, src2)
plt.subplot(231), plt.axis('off'), plt.imshow(src1, 'gray'), plt.title('src1')
plt.subplot(232), plt.axis('off'), plt.imshow(src2, 'gray'), plt.title('src2')
plt.subplot(233), plt.axis('off'), plt.imshow(dst1, 'gray'), plt.title('add')
plt.subplot(234), plt.axis('off'), plt.imshow(dst2, 'gray'), plt.title('addWeighted')
plt.subplot(235), plt.axis('off'), plt.imshow(dst3, 'gray'), plt.title('subtract')
plt.subplot(236), plt.axis('off'), plt.imshow(dst4, 'gray'), plt.title('absdiff')
plt.show()
```

## 🎇 Logical 논리 연산
```Python
import sys
import numpy as np
import cv2
from matplotlib import pyplot as plt

src1 = cv2.imread("./assets/lenna256.bmp", cv2.IMREAD_GRAYSCALE)
src2 = cv2.imread("./assets/square.bmp", cv2.IMREAD_GRAYSCALE)
assert src1 is not None and src2 is not None, "Image load failed!"
dst1 = cv2.bitwise_and(src1, src2)
dst2 = cv2.bitwise_or(src1, src2)
dst3 = cv2.bitwise_xor(src1, src2)
dst4 = cv2.bitwise_not(src1)
plt.subplot(231), plt.axis('off'), plt.imshow(src1, 'gray'), plt.title('src1')
plt.subplot(232), plt.axis('off'), plt.imshow(src2, 'gray'), plt.title('src2')
plt.subplot(233), plt.axis('off'), plt.imshow(dst1, 'gray'), plt.title('bitwise_and')
plt.subplot(234), plt.axis('off'), plt.imshow(dst2, 'gray'), plt.title('bitwise_or')
plt.subplot(235), plt.axis('off'), plt.imshow(dst3, 'gray'), plt.title('bitwise_xor')
plt.subplot(236), plt.axis('off'), plt.imshow(dst4, 'gray'), plt.title('bitwise_not')
plt.show()
```

## 🎇 Blurring 블러링
### 📌 예제: blurring_mean()
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/rose.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
cv2.imshow("src", src)
for ksize in (3, 5, 7):
    dst = cv2.blur(src, (ksize, ksize))
    desc = f"Mean: {ksize}x{ksize}"
    cv2.putText(dst, desc, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1.0, 255, 1, cv2.LINE_AA)
    cv2.imshow("dst", dst)
    cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: blurring_gaussian()
```Python 
import numpy as np
import cv2

src = cv2.imread("./assets/rose.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
cv2.imshow("src", src)
for sigma in range(1, 6):
    dst = cv2.GaussianBlur(src, (0,0), sigma)
    desc = f"Gaussian: sigma = {sigma}"
    cv2.putText(dst, desc, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1.0, 255, 1, cv2.LINE_AA)
    cv2.imshow('dst', dst)
    cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Filter 필터링
### 📌 예제
```Python
import sys
import numpy as np
import cv2

src = cv2.imread("./assets/rose.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
emboss = np.array([[-1, -1, 0],
                    [-1, 0, 1],
                    [0, 1, 1]], np.float32)
dst = cv2.filter2D(src, -1, emboss, delta=128)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Noise 노이즈
### 📌 예제: noise_gaussian()
```Python
import numpy as np
import cv2
import random

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
cv2.imshow("src", src)
for stddev in [10, 20, 30]:
    noise = np.zeros(src.shape, np.int32)
    cv2.randn(noise, 0, stddev)
    dst = cv2.add(src, noise, dtype=cv2.CV_8UC1)
    desc = f"stddev = {stddev}"
    cv2.putText(
        dst, desc, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1.0, 255, 1, cv2.LINE_AA
    )
    cv2.imshow("dst", dst)
    cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: filter_bilateral()
```Python
import numpy as np
import cv2
import random

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
noise = np.zeros(src.shape, np.int32)
cv2.randn(noise, 0, 5)
cv2.add(src, noise, src, dtype=cv2.CV_8UC1)
dst1 = cv2.GaussianBlur(src, (0, 0), 5)
dst2 = cv2.bilateralFilter(src, -1, 10, 5)
cv2.imshow("src", src)
cv2.imshow("dst1", dst1)
cv2.imshow("dst2", dst2)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 예제: filter_median()
```Python
import numpy as np
import cv2
import random

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
for i in range(0, int(src.size / 10)):
    x = random.randint(0, src.shape[1] - 1)
    y = random.randint(0, src.shape[0] - 1)
    src[x, y] = (i % 2) * 255
dst1 = cv2.GaussianBlur(src, (0, 0), 1)
dst2 = cv2.medianBlur(src, 3)
cv2.imshow("src", src)
cv2.imshow("dst1", dst1)
cv2.imshow("dst2", dst2)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Sharpen 선명하게
### 📌 예제
```Python
import sys
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
cv2.imshow("src", src)
for sigma in range(1, 6):
    blurred = cv2.GaussianBlur(src, (0, 0), sigma)
    alpha = 1.0
    dst = cv2.addWeighted(src, 1 + alpha, blurred, -alpha, 0.0)
    desc = f"sigma: {sigma}"
    cv2.putText(
        dst, desc, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1.0, 255, 1, cv2.LINE_AA
    )
    cv2.imshow("dst", dst)
    cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Geometric Transformation 기하학적 변환
### 📌 Affine Transformation

## 🎇 Video FPS

### 📌 모니터 주사율 Hz
➰　수직 주파수, 화면 재생율
- Hz 헤르츠 (주사율 단위)
    - 1초에 몇 개의 화면을 보여주는가
- 모니터 수직 주파수 사양은 60~360Hz까지 다양
    - 고주사율 → 고사양

### 📌 컴퓨터 프레임율 FPS

- 새로고침빈도, 화면재생빈도
- FPS (Frame Per Second)
    - 1초에 몇 개의 프레임(화면)을 만들어 내느냐
- 내장 또는 외장 그래픽과 관련된 사양
    - 고프레임률 → 고사양

> **모니터의 수직 주파수와 그래픽 카드의 새로고침 빈도는 의미 상 동일함**

1. Hz < FPS
    - 화면 찢어짐 **Tearing** (테어링) 발생
        - 화면과 화면이 어긋나서 겹쳐 보임
2. Hz > FPS
    - 화면 끊김 **Stuttering** (스터터링) 발생
        - 화면이 비연속적으로 끊어져 보임

#### ⚡ Input lag 인풋렉
    화면 전송 및 입력 처리 시간 지연 현상

### 📌 예제: 모니터 주사율 확인 후 카메라 FPS 변경

*1. Installation*
```
$ pip install pypiwin32
```

*2. Source code*
```python
from time import time
import win32api
import win32con
import cv2


def get_refresh_rate() -> int:
    """현재 모니터 주사율 반환"""
    dm = win32api.EnumDisplaySettings(None, win32con.ENUM_CURRENT_SETTINGS)
    current_rate = dm.DisplayFrequency
    return current_rate


def main() -> None:
    cap = cv2.VideoCapture(0)

    cap_width = cap.get(cv2.CAP_PROP_FRAME_WIDTH)
    cap_height = cap.get(cv2.CAP_PROP_FRAME_HEIGHT)
    cap_fps = cap.get(cv2.CAP_PROP_FPS)
    print("[fps]", cap_fps)

    monitor_hz = get_refresh_rate()
    print("[hz]", monitor_hz)

    cap.set(cv2.CAP_PROP_FRAME_WIDTH, round(cap_width/3))
    cap.set(cv2.CAP_PROP_FRAME_HEIGHT, round(cap_height/3))
    cap.set(cv2.CAP_PROP_FPS, monitor_hz)

    result = cap.isOpend()

    while result:
        start = time()

        result, frame = cap.read()

        if result:
            cv2.imshow("", frame)
        
        stop = time()

        if cv2.waitKey(1) == 27:
            break
        
        print("[time fps]", round((1./(stop-start)), 3))
        print("[prop fps]", cap.get(cv2.CAP_PROP_FPS))
    
    cap.release()
    cv2.destroyAllWindows()


if __name__ == "__main__":
    main()
```

## 🎇 
### 📌 
```
```
➰　