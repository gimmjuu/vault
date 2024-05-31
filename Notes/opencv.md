---
tags:
  - TAG ONLY CAP
updated:

---

# OpenCV
┌-------------------------┐
│ requirements.txt        │
└-------------------------┘
    python >= 3.6.5
    numpy >= 1.14.5
    matplotlib >= 2.2.2
    opencv-python >= 4.1.0.25
    tensorflow >= 1.31.1

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
### 📌 예제
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
### 📌 Affine
#### ⚡ 예제: affine_transform
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/tekapo.bmp")
assert src is not None, "Image load failed!"
rows = src.shape[0]
cols = src.shape[1]
src_pts = np.array([[0, 0], [cols - 1, 0], [cols - 1, rows - 1]]).astype(
    np.float32
)
dst_pts = np.array(
    [[50, 50], [cols - 100, 100], [cols - 50, rows - 50]]
).astype(np.float32)
affine_mat = cv2.getAffineTransform(src_pts, dst_pts)
dst = cv2.warpAffine(src, affine_mat, (0, 0))
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: affine_translation
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/tekapo.bmp")
assert src is not None, "Image load failed!"
affine_mat = np.array([[1, 0, 150], [0, 1, 100]]).astype(np.float32)
dst = cv2.warpAffine(src, affine_mat, (0, 0))
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: affine_shear
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/tekapo.bmp")
assert src is not None, "Image load failed!"
rows = src.shape[0]
cols = src.shape[1]
mx = 0.3
affine_mat = np.array([[1, mx, 0], [0, 1, 0]]).astype(np.float32)
dst = cv2.warpAffine(src, affine_mat, (int(cols + rows * mx), rows))
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: affine_scale
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/rose.bmp")
assert src is not None, "Image load failed!"
dst1 = cv2.resize(src, (0, 0), fx=4, fy=4, interpolation=cv2.INTER_NEAREST)
dst2 = cv2.resize(src, (1920, 1280))
dst3 = cv2.resize(src, (1920, 1280), interpolation=cv2.INTER_CUBIC)
dst4 = cv2.resize(src, (1920, 1280), interpolation=cv2.INTER_LANCZOS4)
cv2.imshow("src", src)
cv2.imshow("dst1", dst1[400:800, 500:900])
cv2.imshow("dst2", dst2[400:800, 500:900])
cv2.imshow("dst3", dst3[400:800, 500:900])
cv2.imshow("dst4", dst4[400:800, 500:900])
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: affine_rotation
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/tekapo.bmp")
assert src is not None, "Image load failed!"
cp = (src.shape[1] / 2, src.shape[0] / 2)
affine_mat = cv2.getRotationMatrix2D(cp, 20, 1)
dst = cv2.warpAffine(src, affine_mat, (0, 0))
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: affine_flip
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/eastsea.bmp")
assert src is not None, "Image load failed!"
cv2.imshow("src", src)
for flip_code in [1, 0, -1]:
    dst = cv2.flip(src, flip_code)
    desc = f"flipCode: {flip_code}"
    cv2.putText(
        dst,
        desc,
        (10, 30),
        cv2.FONT_HERSHEY_SIMPLEX,
        1.0,
        (255, 0, 0),
        1,
        cv2.LINE_AA,
    )
    cv2.imshow("dst", dst)
    cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 perspective
#### ⚡ 예제
```Python
import numpy as np
import cv2

cnt = 0
src_pts = np.zeros([4, 2], dtype=np.float32)
src = cv2.imread("./assets/card.bmp")
assert src is not None, "Image load failed!"

def on_mouse(event, x, y, flags, param):
    global cnt, src_pts
    if event == cv2.EVENT_LBUTTONDOWN:
        if cnt < 4:
            src_pts[cnt, :] = np.array([x, y]).astype(np.float32)
            cnt += 1
            cv2.circle(src, (x, y), 5, (0, 0, 255), -1)
            cv2.imshow("src", src)
        if cnt == 4:
            w = 200
            h = 300
            dst_pts = np.array(
                [[0, 0], [w - 1, 0], [w - 1, h - 1], [0, h - 1]]
            ).astype(np.float32)
            pers_mat = cv2.getPerspectiveTransform(src_pts, dst_pts)
            dst = cv2.warpPerspective(src, pers_mat, (w, h))
            cv2.imshow("dst", dst)

cv2.namedWindow("src")
cv2.setMouseCallback("src", on_mouse)
cv2.imshow("src", src)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## 🎇 Edges 외곽선 검출과 응용
### 📌 Edges
#### ⚡ 예제: sobel_derivative
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
mx = np.array([[-1, 0, 1],
            [-2, 0, 2],
            [-1, 0, 1]], dtype=np.float32)
my = np.array([[-1, -2, -1],
            [0, 0, 0],
            [1, 2, 1]], dtype=np.float32)
dx = cv2.filter2D(src, -1, mx, delta=128)
dy = cv2.filter2D(src, -1, my, delta=128)
cv2.imshow("src", src)
cv2.imshow("dx", dx)
cv2.imshow("dy", dy)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: sobel_edge
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dx, dy = cv2.Sobel(src, cv2.CV_32F, 1, 0), cv2.Sobel(src, cv2.CV_32F, 0, 1)
fmag = cv2.magnitude(dx, dy)
mag = np.uint8(np.clip(fmag, 0, 255))
_, edge = cv2.threshold(mag, 150, 255, cv2.THRESH_BINARY)
cv2.imshow('src', src)
cv2.imshow('mag', mag)
cv2.imshow('edge', edge)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: canny_edge
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/lenna.bmp", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
dst1 = cv2.Canny(src, 50, 100)
dst2 = cv2.Canny(src, 50, 150)
cv2.imshow('src', src)
cv2.imshow('dst1', dst1)
cv2.imshow('dst2', dst2)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Hough
#### ⚡ 예제: hough_lines
```Python
import numpy as np
import cv2
import math

src = cv2.imread("./assets/building.jpg", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
edge = cv2.Canny(src,50, 150)
lines = cv2.HoughLines(edge, 1, math.pi / 180, 250)
dst = cv2.cvtColor(edge, cv2.COLOR_GRAY2BGR)
if lines is not None:
    for i in range(lines.shape[0]):
        rho = lines[i][0][0]
        theta = lines[i][0][1]
        cos_t = math.cos(theta)
        sin_t = math.sin(theta)
        x0, y0 = rho * cos_t, rho * sin_t
        alpha = 1000
        pt1 = (int(x0 - alpha * sin_t), int(y0 + alpha * cos_t))
        pt2 = (int(x0 + alpha * sin_t), int(y0 - alpha * cos_t))
        cv2.line(dst, pt1, pt2, (0, 0, 255), 2, cv2.LINE_AA)
cv2.imshow('src', src)
cv2.imshow('dst', dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: hough_line_segments
```Python
import numpy as np
import cv2
import math

src = cv2.imread("./assets/building.jpg", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
edge = cv2.Canny(src, 50, 150)
lines = cv2.HoughLinesP(edge, 1, math.pi/180, 160, minLineLength=50, maxLineGap=5)
dst = cv2.cvtColor(edge, cv2.COLOR_GRAY2BGR)
if lines is not None:
    for i in range(lines.shape[0]):
        pt1 = (lines[i][0][0], lines[i][0][1]) # 시작점 x, y
        pt2 = (lines[i][0][2], lines[i][0][3]) # 끝점 x, y
        cv2.line(dst, pt1, pt2, (0, 0, 255), 2, cv2.LINE_AA)

src = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
ret = np.hstack((src, dst))
cv2.imshow("ret", ret)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: hough_circles
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/coins.png", cv2.IMREAD_GRAYSCALE)
assert src is not None, "Image load failed!"
blurred = cv2.blur(src, (3, 3))
circles = cv2.HoughCircles(blurred, cv2.HOUGH_GRADIENT, 1, 50, param1=150, param2=30)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
if circles is not None:
    for i in range(circles.shape[1]):
        cx, cy, radius = circles[0][i]
        cx, cy, radius = round(cx), round(cy), round(radius) # -> 없으면 error 발생!
        cv2.circle(dst, (cx, cy), radius, (0, 0, 255), 2, cv2.LINE_AA)

src = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
ret = np.hstack((src, dst))
cv2.imshow("ret", ret)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Video Operation 컬러 영상 처리
### 📌 Color Operation
#### ⚡ 예제: color_op
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/butterfly.jpg", cv2.IMREAD_COLOR)
assert src is not None, "Image load failed!"
print("src.shape:", src.shape)
# src.shape: (356, 493, 3)
print("src.dtype:", src.dtype)
# src.dtype: uint8
print("The pixel value [B, G, R] at (0, 0) is", src[0, 0])
# The pixel value [B, G, R] at (0, 0) is [47 88 50]
```

#### ⚡ 예제: color_inverse
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/butterfly.jpg", cv2.IMREAD_COLOR)
assert src is not None, "Image load failed!"

dst = np.zeros(src.shape, src.dtype)
for j in range(src.shape[0]):
    for i in range(src.shape[1]):
        p1 = src[j, i]
        p2 = dst[j, i]
        p2[0] = 255 - p1[0]
        p2[1] = 255 - p1[1]
        p2[2] = 255 - p1[2]

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: color_grayscale
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/candies.png", cv2.IMREAD_COLOR)
assert src is not None, "Image load failed!"
b_plane, g_plane, r_plane = cv2.split(src)
cv2.imshow("src", src)
cv2.imshow("b_plane", b_plane)
cv2.imshow("g_plane", g_plane)
cv2.imshow("r_plane", r_plane)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Back Projection
#### ⚡ 예제: BackProj
```Python
import numpy as np
import cv2

# Calculate CrCb histogram from a reference image
ref = cv2.imread("./assets/ref.png", cv2.IMREAD_COLOR)
mask = cv2.imread("./assets/mask.bmp", cv2.IMREAD_GRAYSCALE)
ref_ycrcb = cv2.cvtColor(ref, cv2.COLOR_BGR2YCrCb)

channels = [1, 2]
cr_bins = 128
cb_bins = 128
histSize = [cr_bins, cb_bins]
cr_range = [0, 256]
cb_range = [0, 256]
ranges = cr_range + cb_range

hist = cv2.calcHist([ref_ycrcb], channels, mask, histSize, ranges)

# Apply histogram backprojection to an input image
src = cv2.imread("./assets/kids.png", cv2.IMREAD_COLOR)
src_ycrcb = cv2.cvtColor(src, cv2.COLOR_BGR2YCrCb)

backproj = cv2.calcBackProject([src_ycrcb], channels, hist, ranges, 1)

cv2.imshow("src", src)
cv2.imshow("backproj", backproj)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Color Equalization
#### ⚡ 예제: coloreq
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/pepper.bmp", cv2.IMREAD_COLOR)
src_ycrcb = cv2.cvtColor(src, cv2.COLOR_BGR2YCrCb)
ycrcb_planes = cv2.split(src_ycrcb)
ycrcb_planes = list(ycrcb_planes)
ycrcb_planes[0] = cv2.equalizeHist(ycrcb_planes[0])
dst_ycrcb = cv2.merge(ycrcb_planes)
dst = cv2.cvtColor(dst_ycrcb, cv2.COLOR_YCrCb2BGR)

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 inRange
#### ⚡ 예제: inrange
```Python
import numpy as np
import cv2

def on_hue_changed(_=None):
    lower_hue = cv2.getTrackbarPos("Lower Hue", "mask")
    upper_hue = cv2.getTrackbarPos("Upper Hue", "mask")

    lowerb = (lower_hue, 100, 0)
    upperb = (upper_hue, 255, 255)
    mask = cv2.inRange(src_hsv, lowerb, upperb)

    cv2.imshow("mask", mask)

global src_hsv

src = cv2.imread("./assets/candies.png", cv2.IMREAD_COLOR)
src_hsv = cv2.cvtColor(src, cv2.COLOR_BGR2HSV)

cv2.imshow("src", src)
cv2.namedWindow("mask")
cv2.createTrackbar("Lower Hue", "mask", 40, 179, on_hue_changed)
cv2.createTrackbar("Upper Hue", "mask", 80, 179, on_hue_changed)
on_hue_changed(0)

cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 이진화와 모폴로지
### 📌 Abaptive
#### ⚡ 예제: adaptive
```Python
import cv2

src = cv2.imread("./assets/sudoku.jpg", cv2.IMREAD_GRAYSCALE)

def on_trackbar(pos):
    bsize = pos
    
    if bsize % 2 == 0: bsize -= bsize
    if bsize < 3: bsize = 3
    
    dst = cv2.adaptiveThreshold(src, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, bsize, 5)
    
    cv2.imshow("dst", dst)
    
cv2.imshow("src", src)
cv2.namedWindow("dst")
cv2.createTrackbar("Block Size", "dst", 0, 200, on_trackbar)
cv2.setTrackbarPos("Block Size", "dst", 11)

cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Morphology
#### ⚡ 예제: erode_dilate
```Python
import cv2
from matplotlib import pyplot as plt

src = read_image("./assets/milkdrop.bmp", cv2.IMREAD_GRAYSCALE)
_, src_bin = cv2.threshold(src, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
dst1 = cv2.erode(src_bin, None)
dst2 = cv2.dilate(src_bin, None)

plt.subplot(221), plt.axis('off'), plt.imshow(src, "gray"), plt.title("src")
plt.subplot(222), plt.axis('off'), plt.imshow(src_bin, "gray"), plt.title("src_bin")
plt.subplot(223), plt.axis('off'), plt.imshow(dst1, "gray"), plt.title("erode")
plt.subplot(224), plt.axis('off'), plt.imshow(dst2, "gray"), plt.title("dilate")
plt.show()
```

#### ⚡ 예제: open_close
```Python
import cv2
from matplotlib import pyplot as plt

src = read_image("./assets/milkdrop.bmp", cv2.IMREAD_GRAYSCALE)
_, src_bin = cv2.threshold(src, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
dst1 = cv2.morphologyEx(src_bin, cv2.MORPH_OPEN, None)
dst2 = cv2.morphologyEx(src_bin, cv2.MORPH_CLOSE, None)

plt.subplot(221), plt.axis('off'), plt.imshow(src, "gray"), plt.title("src")
plt.subplot(222), plt.axis('off'), plt.imshow(src_bin, "gray"), plt.title("src_bin")
plt.subplot(223), plt.axis('off'), plt.imshow(dst1, "gray"), plt.title("erode")
plt.subplot(224), plt.axis('off'), plt.imshow(dst2, "gray"), plt.title("dilate")
plt.show()
```

### 📌 Threshold
#### ⚡ 예제: threshold
```Python
import cv2

src = read_image("./assets/neutrophils.png", cv2.IMREAD_GRAYSCALE)
cv2.imshow('src', src)

def on_threshold(pos):
    _, dst = cv2.threshold(src, pos, 255, cv2.THRESH_BINARY)
    cv2.imshow("dst", dst)
    
cv2.namedWindow("dst")
cv2.createTrackbar("Threshold", "dst", 0, 255, on_threshold)
cv2.setTrackbarPos("Threshold", "dst", 128)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## 🎇 Labeling and Contouring 레이블링과 외곽선 검출
### 📌 findcts
#### ⚡ 예제: contours_basic
```Python
import cv2
import random

src = cv2.imread("./assets/contours.bmp", cv2.IMREAD_GRAYSCALE)

contours, _ = cv2.findContours(src, cv2.RETR_LIST, cv2.CHAIN_APPROX_NONE)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
for i, _ in enumerate(contours):
    c = (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255))
    cv2.drawContours(dst, contours, i, c, 2)

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: contours_hier
```Python
import cv2
import random

src = cv2.imread("./assets/contours.bmp", cv2.IMREAD_GRAYSCALE)

contours, hierarchy = cv2.findContours(src, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_SIMPLE)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
idx = 0
while idx >= 0:
    b, g, r = random.randint(0, 255), random.randint(0, 255), random.randint(0, 255)
    c = (b, g, r)
    cv2.drawContours(dst, contours, idx, c, -1, cv2.LINE_8, hierarchy)
    idx = hierarchy[0, idx, 0]

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Labeling
#### ⚡ 예제: labeling_basic
```Python
import numpy as np
import cv2

src = np.array([[0, 0, 1, 1, 0, 0, 0, 0],
                [1, 1, 1, 1, 0, 0, 1, 0],
                [1, 1, 1, 1, 0, 0, 0, 0],
                [0, 0, 0, 0, 0, 1, 1, 0],
                [0, 0, 0, 1, 1, 1, 1, 0],
                [0, 0, 0, 1, 0, 0, 1, 0],
                [0, 0, 1, 1, 1, 1, 1, 0],
                [0, 0, 0, 0, 0, 0, 0, 0]]).astype(np.uint8)
src *= 255
cnt, labels = cv2.connectedComponents(src)
print("src:\n", src)
# src:
#  [[  0   0 255 255   0   0   0   0]
#  [255 255 255 255   0   0 255   0]
#  [255 255 255 255   0   0   0   0]
#  [  0   0   0   0   0 255 255   0]
#  [  0   0   0 255 255 255 255   0]
#  [  0   0   0 255   0   0 255   0]
#  [  0   0 255 255 255 255 255   0]
#  [  0   0   0   0   0   0   0   0]]
print("labels:\n", labels)
# labels:
#  [[0 0 1 1 0 0 0 0]
#  [1 1 1 1 0 0 2 0]
#  [1 1 1 1 0 0 0 0]
#  [0 0 0 0 0 3 3 0]
#  [0 0 0 3 3 3 3 0]
#  [0 0 0 3 0 0 3 0]
#  [0 0 3 3 3 3 3 0]
#  [0 0 0 0 0 0 0 0]]
print("number of labels:", cnt)
# number of labels: 4
```

#### ⚡ 예제: labeling_stats
```Python
import numpy as np
import cv2

src = cv2.imread("./assets/keyboard.bmp", cv2.IMREAD_GRAYSCALE)

_, src_bin = cv2.threshold(src, 0, 255, cv2.THRESH_BINARY | cv2.THRESH_OTSU)
cnt, labels, stats, centroids = cv2.connectedComponentsWithStats(src_bin)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
for i in range(1, cnt):
    x, y, w, h, area = stats[i]
    if area < 20: continue
    pt1 = x, y
    pt2 = x + w, y + h
    cv2.rectangle(dst, pt1, pt2, (0, 255, 255))

cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: polygon
```Python
import cv2
import math

def setLabel(img, pts,label):
    x, y, w, h = cv2.boundingRect(pts)
    pt1 = x, y
    pt2 = x + w, y + h
    cv2.rectangle(img, pt1, pt2, (0, 0, 255), 1)
    cv2.putText(img, label, pt1, cv2.FONT_HERSHEY_PLAIN, 1, (0, 0, 255))
    
img = cv2.imread("./assets/polygon.bmp", cv2.IMREAD_COLOR)
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
_, img_bin = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY_INV | cv2.THRESH_OTSU)
contours, _ = cv2.findContours(img_bin, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)
for pts in contours:
    if cv2.contourArea(pts) < 400:
        continue
    approx = cv2.approxPolyDP(pts, cv2.arcLength(pts, True)*0.02, True)
    vtc = len(approx)
    if vtc == 3: setLabel(img, pts, "TRI")
    elif vtc == 4: setLabel(img, pts, "RECT")
    else:
        lenth = cv2.arcLength(pts, True)
        area = cv2.contourArea(pts)
        ratio = 4. * math.pi * area / (lenth * lenth)
        if ratio > 0.85: setLabel(img, pts, "CIR")
cv2.imshow("img", img)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Object Detection 객체 검출
### 📌 Cascade
#### ⚡ 예제: detect_face
```Python
import cv2

src = cv2.imread("./assets/kids.png", cv2.IMREAD_COLOR)

classifier = cv2.CascadeClassifier("./data/haarcascade_frontalface_default.xml")
if classifier.empty():
    print("XML load failed!")
    return
faces = classifier.detectMultiScale(src)
for (x, y, w, h) in faces:
    cv2.rectangle(src, (x, y), (x + w, y + h), (255, 0, 255), 2)

cv2.imshow("src", src)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: detect_eyes
```Python
import cv2

src = cv2.imread("./assets/kids.png", cv2.IMREAD_COLOR)

face_classifier = cv2.CascadeClassifier("./data/haarcascade_frontalface_default.xml")
eye_classifier = cv2.CascadeClassifier("./data/haarcascade_eye.xml")
if face_classifier.empty() or eye_classifier.empty():
    print("XML load failed!")
    return
faces = face_classifier.detectMultiScale(src)
for (x1, y1, w1, h1) in faces:
    cv2.rectangle(src, (x1, y1), (x1 + w1, y1 + h1), (255, 0, 255), 2)
    faceROI = src[y1:y1 + h1, x1:x1 + w1]
    eyes = eye_classifier.detectMultiScale(faceROI)
    for (x2, y2, w2, h2) in eyes:
        center = (int(x2 + w2 / 2), int(y2 + h2 / 2))
        cv2.circle(faceROI, center, int(w2 / 2), (255, 0, 0), 2, cv2.LINE_AA)

cv2.imshow("src", src)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 HOG
#### ⚡ 예제: hog
```Python
import cv2
import random

cap = cv2.VideoCapture("./assets/vtest.avi")
if not cap.isOpened():
    print("Video open failed!")
    return
hog = cv2.HOGDescriptor()
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())
while True:
    ret, frame = cap.read()
    if not ret:
        break
    detected, _ = hog.detectMultiScale(frame)
    for (x, y, w, h) in detected:
        c = (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255))
        cv2.rectangle(frame, (x, y), (x + w, y + h), c, 3)
    cv2.imshow("frame", frame)
    if cv2.waitKey(10) == 27:
        break
cv2.destroyAllWindows()
```

### 📌 QR Code
#### ⚡ 예제: qrcode
```Python
import sys
import numpy as np
import cv2

cap = cv2.VideoCapture(0)
if not cap.isOpened():
    print("Video open failed!")
    sys.exit()

detector = cv2.QRCodeDetector()
while True:
    ret, frame = cap.read()
    if not ret:
        print("Frame load failed!")
        break
    info, points, _ = detector.detectAndDecode(frame)
    if points is not None:
        points = np.array(points, dtype=np.int32).reshape(4, 2)
        cv2.polylines(frame, [points], True, (0, 0, 255), 2)
    if len(info) > 0:
        cv2.putText(frame, info, (10, 30), cv2.FONT_HERSHEY_DUPLEX, 1, (0, 0, 255), lineType=cv2.LINE_AA)
    cv2.imshow("frame", frame)
    if cv2.waitKey(1) == 27:
        break

cv2.destroyAllWindows()
```

### 📌 Template
#### ⚡ 예제: 
```Python
import numpy as np
import cv2

img = cv2.imread("./assets/circuit.bmp", cv2.IMREAD_COLOR)
templ = cv2.imread("./assets/crystal.bmp", cv2.IMREAD_COLOR)

img = img + (50, 50, 50)
noise = np.zeros(img.shape, np.int32)
cv2.randn(noise, 0, 10)
img = cv2.add(img, noise, dtype=cv2.CV_8UC3)
res = cv2.matchTemplate(img, templ, cv2.TM_CCOEFF_NORMED)
res_norm = cv2.normalize(res, None, 0, 255, cv2.NORM_MINMAX, cv2.CV_8U)
_, maxv, _, maxloc = cv2.minMaxLoc(res)
print("maxv:", maxv)
(th, tw) = templ.shape[:2]
cv2.rectangle(img, maxloc, (maxloc[0] + tw, maxloc[1] + th), (0, 0, 255), 2)

cv2.imshow("templ", templ)    
cv2.imshow("res_norm", res_norm)    
cv2.imshow("img", img)   
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Corners and Matching Keypoints 지역 특징점 검출과 매칭
### 📌 Corners
#### ⚡ 예제: corner_harris
```Python
import cv2

src = cv2.imread("./assets/building.jpg", cv2.IMREAD_GRAYSCALE)

harris = cv2.cornerHarris(src, 3, 3, 0.04)
harris_norm = cv2.normalize(harris, None, 0, 255, cv2.NORM_MINMAX, cv2.CV_8U)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
for y in range(harris_norm.shape[0]):
    for x in range(harris_norm.shape[1]):
        if harris_norm[y, x] > 120:
            if (harris[y, x] > harris[y-1, x] and
                harris[y, x] > harris[y-1, x] and
                harris[y, x] > harris[y-1, x] and
                harris[y, x] > harris[y-1, x]):
                cv2.circle(dst, (x, y), 5, (0, 0, 255), 2)

cv2.imshow("src", src)
cv2.imshow("harris_norm", harris_norm)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: corner_fast
```Python
import cv2

src = cv2.imread("./assets/building.jpg", cv2.IMREAD_GRAYSCALE)
fast = cv2.FastFeatureDetector_create(60)
keypoints = fast.detect(src)
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)
for kp in keypoints:
    pt = (int(kp.pt[0]), int(kp.pt[1]))
    cv2.circle(dst, pt, 5, (0, 0, 255), 2)
ret = stack_two_images(src, dst)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Keypoint
#### ⚡ 예제: 
```Python
import cv2

src = cv2.imread("./assets/box_in_scene.png", cv2.IMREAD_GRAYSCALE)
orb = cv2.ORB_create()
keypoints = orb.detect(src)
keypoints, desc = orb.compute(src, keypoints)
print("len(keypoints):", len(keypoints))
# len(keypoints): 500
print("desc.shape:", desc.shape)
# desc.shape: (500, 32)
dst = cv2.drawKeypoints(src, keypoints, None, (-1, -1, -1), cv2.DrawMatchesFlags_DRAW_RICH_KEYPOINTS)
ret = stack_two_images(src, dst)
cv2.imshow("src", src)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Match
#### ⚡ 예제: keypoit_matching
```Python
import numpy as np
import cv2

src1 = cv2.imread("./assets/box.png", cv2.IMREAD_GRAYSCALE)
src2 = cv2.imread("./assets/box_in_scene.png", cv2.IMREAD_GRAYSCALE)
orb = cv2.ORB_create()
keypoints1, desc1 = orb.detectAndCompute(src1, None)
keypoints2, desc2 = orb.detectAndCompute(src2, None)
print("desc1.shape:", desc1.shape)
print("desc2.shape:", desc2.shape)
matcher = cv2.BFMatcher_create(cv2.NORM_HAMMING)
matches = matcher.match(desc1, desc2)
dst = cv2.drawMatches(src1, keypoints1, src2, keypoints2, matches, None)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 good_matching
#### ⚡ 예제: 
```Python
import cv2

src1 = cv2.imread("./assets/box.png", cv2.IMREAD_GRAYSCALE)
src2 = cv2.imread("./assets/box_in_scene.png", cv2.IMREAD_GRAYSCALE)
orb = cv2.ORB_create()
keypoints1, desc1 = orb.detectAndCompute(src1, None)
keypoints2, desc2 = orb.detectAndCompute(src2, None)
matcher = cv2.BFMatcher_create(cv2.NORM_HAMMING)
matches = matcher.match(desc1, desc2)
matches = sorted(matches, key=lambda x: x.distance)
good_matches = matches[:50]
dst = cv2.drawMatches(
    src1, keypoints1, src2, keypoints2, good_matches, None, flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS
    )
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

#### ⚡ 예제: find_homography
```Python
import numpy as np
import cv2

src1 = cv2.imread("./assets/box.png", cv2.IMREAD_GRAYSCALE)
src2 = cv2.imread("./assets/box_in_scene.png", cv2.IMREAD_GRAYSCALE)
orb = cv2.ORB_create()
keypoints1, desc1 = orb.detectAndCompute(src1, None)
keypoints2, desc2 = orb.detectAndCompute(src2, None)
matcher = cv2.BFMatcher_create(cv2.NORM_HAMMING)
matches = matcher.match(desc1, desc2)
matches = sorted(matches, key=lambda x: x.distance)
good_matches = matches[:50]
dst = cv2.drawMatches(src1, keypoints1, src2, keypoints2, good_matches, None, flags=cv2.DrawMatchesFlags_NOT_DRAW_SINGLE_POINTS)
pts1 = np.array([keypoints1[m.queryIdx].pt for m in good_matches]).reshape(-1, 1, 2).astype(np.float32)
pts2 = np.array([keypoints2[m.trainIdx].pt for m in good_matches]).reshape(-1, 1, 2).astype(np.float32)
H, _ = cv2.findHomography(pts1, pts2, cv2.RANSAC)
(h, w) = src1.shape[:2]
corners1 = np.array([[0, 0], [0, h-1], [w-1, h-1], [w-1, 0]]).reshape(-1, 1, 2).astype(np.float32)
corners2 = cv2.perspectiveTransform(corners1, H)
corners2 = corners2 + np.float32([w, 0])
cv2.polylines(dst, [np.int32(corners2)], True, (0, 255, 0), 2, cv2.LINE_AA)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 Stitch
#### ⚡ 예제: stitching 
```Python
import sys
import cv2

argc = len(sys.argv)
if argc < 3:
    print('Usage: stitching.exe <image_file1> <image_file2> [<image_file3> ...]')
    sys.exit()
imgs = list()
for i in range(1, argc):
    img = read_image(sys.argv[i])
    imgs.append(img)
stitcher = cv2.Stitcher_create()
status, dst = stitcher.stitch(imgs)
if status != cv2.Stitcher_OK:
    print("Error on stitching!")
    sys.exit()
cv2.imwrite("./saves/result.jpg", dst)
cv2.imshow("dst", dst)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Machine Learning 머신러닝
### 📌 KNN 최인접이웃
#### ⚡ 예제 1: knndigits
```Python
import numpy as np
import cv2

oldx, oldy = -1, -1

def on_mouse(event, x, y, flags, _):
    global oldx, oldy
    if event == cv2.EVENT_LBUTTONDOWN:
        oldx, oldy = x, y
    elif event == cv2.EVENT_LBUTTONUP:
        oldx, oldy = -1, -1
    elif event == cv2.EVENT_MOUSEMOVE:
        if flags & cv2.EVENT_FLAG_LBUTTON:
            cv2.line(img, (oldx, oldy), (x, y), (255, 255, 255), 40, cv2.LINE_AA)
            oldx, oldy = x, y
            cv2.imshow("img", img)

# Generate training samples and labels
digits = cv2.imread("./assets/digits.png", cv2.IMREAD_GRAYSCALE)
h, w = digits.shape[:2]
cells = [np.hsplit(row, w//20) for row in np.vsplit(digits, h//20)]
cells = np.array(cells)
train_images = cells.reshape(-1, 400).astype(np.float32)
train_labels = np.repeat(np.arange(10), len(train_images)/10)

# Training KNN
knn = cv2.ml.KNearest_create()
knn.train(train_images, cv2.ml.ROW_SAMPLE, train_labels)

# Tests
img = np.zeros((400, 400), np.uint8)

cv2.imshow("img", img)
cv2.setMouseCallback("img", on_mouse)

while True:
    c = cv2.waitKey()
    if c == 27:
        break
    elif c == ord(' '):
        img_resize = cv2.resize(img, (20, 20), interpolation=cv2.INTER_AREA)
        img_flatten = img_resize.reshape(-1, 400).astype(np.float32)
        _ret, res, _, _ = knn.findNearest(img_flatten, 3)
        print(int(res[0, 0]))
        img.fill(0)
        cv2.imshow("img", img)
cv2.destroyAllWindows()
```

#### ⚡ 예제2: knnplane
```Python
import numpy as np
import cv2

train = []
label = []
k_value = 1

def on_k_changed(pos):
    global k_value
    k_value = pos
    if k_value < 1:
        k_value = 1
    trainAndDisplay()
    
def addPoint(x, y, c):
    train.append([x, y])
    label.append([c])
    
def trainAndDisplay():
    train_array = np.array(train).astype(np.float32)
    label_array = np.array(label)
    knn.train(train_array, cv2.ml.ROW_SAMPLE, label_array)
    for j in range(img.shape[0]):
        for i in range(img.shape[1]):
            sample = np.array([[i, j]]).astype(np.float32)
            ret, res, _, _ = knn.findNearest(sample, k_value)
            response = int(res[0, 0])
            if response == 0: img[j, i] = (128, 128, 255)
            elif response == 1: img[j, i] = (128, 255, 128)
            elif response == 2: img[j, i] = (255, 128, 128)
    for i in range(len(train)):
        x, y = train[i]
        l = label[i][0]
        if l == 0: cv2.circle(img, (x, y), 5, (0, 0, 128), -1, cv2.LINE_AA)
        elif l == 1: cv2.circle(img, (x, y), 5, (0, 128, 0), -1, cv2.LINE_AA)
        elif l == 2: cv2.circle(img, (x, y), 5, (128, 0, 0), -1, cv2.LINE_AA)
    cv2.imshow('knn', img)

img = np.zeros((500, 500, 3), np.uint8)
knn = cv2.ml.KNearest_create()
NUM = 30
rn = np.zeros((NUM, 2), np.int32)

cv2.randn(rn, 0, 50)
for i in range(NUM):
    addPoint(rn[i, 0] + 150, rn[i, 1] + 150, 0)

cv2.randn(rn, 0, 50)
for i in range(NUM):
    addPoint(rn[i, 0] + 350, rn[i, 1] + 150, 1)

cv2.randn(rn, 0, 70)
for i in range(NUM):
    addPoint(rn[i, 0] + 250, rn[i, 1] + 400, 2)

cv2.namedWindow('knn')
cv2.createTrackbar('k_value', 'knn', k_value, 5, on_k_changed)
trainAndDisplay()

cv2.imshow("knn", img)
cv2.waitKey()
cv2.destroyAllWindows()
```

### 📌 SVM 서포트 벡터 머신
#### ⚡ 예제 1: svmdigits
```Python
import sys
import numpy as np
import cv2

oldx, oldy = -1, -1


def on_mouse(event, x, y, flags, _):
    global oldx, oldy

    if event == cv2.EVENT_LBUTTONDOWN:
        oldx, oldy = x, y
    elif event == cv2.EVENT_LBUTTONUP:
        oldx, oldy = -1, -1
    elif event == cv2.EVENT_MOUSEMOVE:
        if flags & cv2.EVENT_FLAG_LBUTTON:
            cv2.line(img, (oldx, oldy), (x, y), (255, 255, 255), 40, cv2.LINE_AA)
            oldx, oldy = x, y
            cv2.imshow('img', img)

# Generate training samples and labels
digits = cv2.imread('./assets/digits.png', cv2.IMREAD_GRAYSCALE)
if digits is None:
    print('Image load failed!')
    sys.exit()

h, w = digits.shape[:2]
hog = cv2.HOGDescriptor((20, 20), (10, 10), (5, 5), (5, 5), 9)

cells = [np.hsplit(row, w//20) for row in np.vsplit(digits, h//20)]
cells = np.array(cells)
cells = cells.reshape(-1, 20, 20)

desc = []
for img in cells:
    dd = hog.compute(img)
    desc.append(dd)

train_desc = np.array(desc).squeeze().astype(np.float32)
train_labels = np.repeat(np.arange(10), len(train_desc)/10)

# Training SVM
svm = cv2.ml.SVM_create()
svm.setType(cv2.ml.SVM_C_SVC)
svm.setKernel(cv2.ml.SVM_RBF)
svm.setC(2.5)
svm.setGamma(0.50625)
svm.train(train_desc, cv2.ml.ROW_SAMPLE, train_labels)

# Tests
img = np.zeros((400, 400), np.uint8)
cv2.imshow('img', img)
cv2.setMouseCallback('img', on_mouse)

while True:
    c = cv2.waitKey()
    if c == 27:
        break
    elif c == ord(' '):
        img_resize = cv2.resize(img, (20, 20), interpolation=cv2.INTER_AREA)
        desc = hog.compute(img_resize)
        test_desc = np.array(desc).astype(np.float32)
        _, res = svm.predict(test_desc.T)
        print(int(res[0, 0]))

        img.fill(0)
        cv2.imshow('img', img)
cv2.destroyAllWindows()
```

#### ⚡ 예제 2: svmplane
```Python
import numpy as np
import cv2

train = np.array([[150, 200], [200, 250],
                [100, 250], [150, 300],
                [350, 100], [400, 200],
                [400, 300], [350, 400]]).astype(np.float32)
label = np.array([0, 0, 0, 0, 1, 1, 1, 1])
svm = cv2.ml.SVM_create()
svm.setType(cv2.ml.SVM_C_SVC)
svm.setKernel(cv2.ml.SVM_RBF)
# svm.setKernel(cv2.ml.SVN_LINEAR)
svm.trainAuto(train, cv2.ml.ROW_SAMPLE, label)
img = np.zeros((500, 500, 3), np.uint8)
for j in range(img.shape[0]):
    for i in range(img.shape[1]):
        test = np.array([[i, j]], dtype=np.float32)
        _, res = svm.predict(test)
        if res == 0: img[j, i] = (128, 128, 255)
        elif res == 1: img[j, i] = (128, 255, 128)
color = [(0, 0, 128), (0, 128, 0)]
for i in range(train.shape[0]):
    x = train[i, 0]
    y = train[i, 1]
    l = label[i]
    cv2.circle(img, (int(x), int(y)), 5, color[l], -1, cv2.LINE_AA)

cv2.imshow("svm", img)
cv2.waitKey()
cv2.destroyAllWindows()
```

## 🎇 Deep Learning with OpenCV 딥러닝
### 📌 Classify
#### ⚡ 예제: dnn_classification
```Python
import numpy as np
import cv2

filename = "./assets/space_shuttle.jpg"
img = read_image(filename)

# Load network
net = cv2.dnn.readNet("./data/bvlc_googlenet.caffemodel", "./data/deploy.prototxt")
if net.empty():
    print("Network load failed!")
    return

# Load class names
classNames = None
with open("./data/classification_classes_ILSVRC2012.txt", "rt") as f:
    classNames = f.read().rstrip('\n').split("\n")

# Inference
inputBlob = cv2.dnn.blobFromImage(img, 1, (224, 224), (104, 117, 123))
net.setInput(inputBlob, "data")
prob = net.forward()

# Check results & Display
out = prob.flatten()
classId = np.argmax(out)
confidence = out[classId]
str = f"{classNames[classId]} ({confidence * 100:4.2f}%)"
cv2.putText(img, str, (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 0, 255), 1, cv2.LINE_AA)
cv2.imshow("img", img)
```

#### ⚡ 예제: dnn_face_detection
```Python
import sys
import cv2

model = './data/res10_300x300_ssd_iter_140000_fp16.caffemodel'
config = './data/deploy.prototxt'
# model = './data/opencv_face_detector_uint8.pb'
# config = './data/opencv_face_detector.pbtxt'

cap = cv2.VideoCapture(0)
if not cap.isOpened():
    print('Camera open failed!')
    sys.exit()

net = cv2.dnn.readNet(model, config)
if net.empty():
    print('Net open failed!')
    sys.exit()

while True:
    _, frame = cap.read()
    if frame is None: break

    blob = cv2.dnn.blobFromImage(frame, 1, (300, 300), (104, 177, 123))
    net.setInput(blob)
    detect = net.forward()
    (h, w) = frame.shape[:2]
    detect = detect[0, 0, :, :]
    
    for i in range(detect.shape[0]):
        confidence = detect[i, 2]
        if confidence < 0.5: break
        x1 = int(detect[i, 3] * w)
        y1 = int(detect[i, 4] * h)
        x2 = int(detect[i, 5] * w)
        y2 = int(detect[i, 6] * h)
        cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0))
        label = 'Face: %4.3f' % confidence
        cv2.putText(frame, label, (x1, y1 - 1), cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 1, cv2.LINE_AA)
    cv2.imshow('frame', frame)

    if cv2.waitKey(1) == 27:
        break
cv2.destroyAllWindows()
```

#### ⚡ 예제: dnn_mnist
```Python
import sys
import numpy as np
import cv2

oldx, oldy = -1, -1

def on_mouse(event, x, y, flags, _):
    global oldx, oldy
    if event == cv2.EVENT_LBUTTONDOWN:
        oldx, oldy = x, y
    elif event == cv2.EVENT_LBUTTONUP:
        oldx, oldy = -1, -1
    elif event == cv2.EVENT_MOUSEMOVE:
        if flags & cv2.EVENT_FLAG_LBUTTON:
            cv2.line(img, (oldx, oldy), (x, y), (255, 255, 255), 40, cv2.LINE_AA)
            oldx, oldy = x, y
            cv2.imshow('img', img)

net = cv2.dnn.readNet('./data/mnist_cnn.pb')
if net.empty():
    print('Network load failed!')
    sys.exit()

img = np.zeros((400, 400), np.uint8)
cv2.imshow('img', img)
cv2.setMouseCallback('img', on_mouse)

while True:
    c = cv2.waitKey()
    if c == 27:
        break
    elif c == ord(' '):
        blob = cv2.dnn.blobFromImage(img, 1/255., (28, 28))
        net.setInput(blob)
        prob = net.forward()
        _, maxVal, _, maxLoc = cv2.minMaxLoc(prob)
        digit = maxLoc[0]
        print('%d (%f)' % (digit, maxVal * 100))
        img.fill(0)
        cv2.imshow('img', img)
cv2.destroyAllWindows()
```

#### ⚡ 예제: cnn_mnist
```Python
import tensorflow as tf
from tensorflow.keras.layers import (
    Conv2D,
    MaxPooling2D,
    Flatten,
    Dense,
    Softmax,
    ReLU,
)
from tensorflow.python.framework import graph_util
from tensorflow.python.platform import gfile
import tf.compat.v1 as tf1

tf.compat.v1.disable_eager_execution()
tf.compat.v1.logging.set_verbosity(tf.compat.v1.logging.ERROR)

mnist = tf.keras.datasets.mnist
# mnist = input_data.read_data_sets("./MNIST_data/", one_hot=True)

# hyper parameters
learning_rate = 0.001
training_epochs = 20
batch_size = 100

# Model configuration
X = tf1.placeholder(tf.float32, [None, 28, 28, 1], name="data")
Y = tf1.placeholder(tf.float32, [None, 10])

print(type(X), X)
conv1 = Conv2D(X, 32, [3, 3], padding="same", activation=ReLU)
pool1 = MaxPooling2D(conv1, [2, 2], strides=2, padding="same")
conv2 = Conv2D(pool1, 64, [3, 3], padding="same", activation=ReLU)
pool2 = MaxPooling2D(conv2, [2, 2], strides=2, padding="same")
flat3 = Flatten(pool2)
dense3 = Dense(flat3, 256, activation=ReLU)
logits = Dense(dense3, 10, activation=None)
final_tensor = Softmax(logits, name="prob")
cost = tf.reduce_mean(
    tf.nn.softmax_cross_entropy_with_logits_v2(labels=Y, logits=logits)
)
optimizer = tf.train.AdamOptimizer(learning_rate).minimize(cost)

# conv1 = tf1.layers.conv2d(X, 32, [3, 3], padding="same", activation=tf.nn.relu)
# pool1 = tf1.layers.max_pooling2d(conv1, [2, 2], strides=2, padding="same")
# conv2 = tf1.layers.conv2d(pool1, 64, [3, 3], padding="same", activation=tf.nn.relu)
# pool2 = tf1.layers.max_pooling2d(conv2, [2, 2], strides=2, padding="same")
# flat3 = tf1.layers.flatten(pool2)
# dense3 = tf1.layers.dense(flat3, 256, activation=tf.nn.relu)
# logits = tf1.layers.dense(dense3, 10, activation=None)
# final_tensor = tf.nn.softmax(logits, name='prob')
# cost = tf.reduce_mean(input_tensor=tf.nn.softmax_cross_entropy_with_logits(labels=Y, logits=logits))
# optimizer = tf1.train.AdamOptimizer(learning_rate).minimize(cost)

# Training
with tf.Session() as sess:
# with tf1.Session() as sess:
    sess.run(tf.global_variables_initializer())
    # sess.run(tf1.global_variables_initializer())
    total_batch = int(mnist.train.num_examples / batch_size)
    print("Start learning!")
    for epoch in range(training_epochs):
        total_cost = 0
        for i in range(total_batch):
            batch_xs, batch_ys = mnist.train.next_batch(batch_size)
            batch_xs = batch_xs.reshape(-1, 28, 28, 1)
            _, cost_val = sess.run(
                [optimizer, cost], feed_dict={X: batch_xs, Y: batch_ys}
            )
            total_cost += cost_val
        print(
            "Epoch:",
            "%04d" % (epoch + 1),
            "Avg. cost = ",
            "{:.4f}".format(total_cost / total_batch),
        )
    print("Learning finished!")

    # Freeze variables and save pb file
    output_graph_def = graph_util.convert_variables_to_constants(
        sess, sess.graph_def, ["prob"]
    )
    with gfile.FastGFile("./save/mnist_cnn.pb", "wb", encoding="utf8") as f:
        f.write(output_graph_def.SerializeToString())
    print("Save done!")
```

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

## 🎇 Image Utils
### 📌 common import
```Python
import numpy as np
import cv2
```

### 📌 def read_image()
```Python
def read_image(image_path: str, t_option: int = cv2.IMREAD_COLOR):
    image = cv2.imread(image_path, t_option)
    assert image is not None, "Image load failed!"

    return image
```

### 📌 def show_image()
```Python
def show_image(image: np.ndarray, title: str = "IMAGE") -> None:
    """
    about imshow() with all key press action
    """
    cv2.imshow(title, image)
    cv2.waitKey()
    cv2.destroyAllWindows()
```

### 📌 def resize_image_using_aspect_ratio()
```Python
def resize_image_using_aspect_ratio(
    image: np.ndarray, t_size: int, isWidth: bool = True
):
    """same function with 'imutils.resize(img, width=...)'"""
    if isWidth:
        # --- 너비 기준 연산
        t_width = t_size
        aspect_ratio = float(t_width) / image.shape[1]  # 종횡비
        t_height = int(image.shape[0] * aspect_ratio)
    else:
        # --- 높이 기준 연산
        t_height = t_size
        aspect_ratio = float(t_height) / image.shape[0]
        t_width = int(image.shape[1] * aspect_ratio)

    resized_result = cv2.resize(
        image, (t_width, t_height), interpolation=cv2.INTER_AREA
    )
    # resized_result = cv2.resize(
    #    image, (t_width, t_height), interpolation=cv2.INTER_LINEAR
    # )
    return resized_result
```

### 📌 def stack_two_images()
```Python
def stack_two_images(image1: np.ndarray, image2: np.ndarray, t_axis: int=1):
    '''
    params: If axis is 1, execute horizontal concat else if axis is 0, execute vertical concat
    result -> concat_image
    '''
    shape1 = image1.shape
    shape2 = image2.shape
    
    H1, W1 = shape1[0], shape1[1]
    H2, W2 = shape2[0], shape2[1]
    
    channel1 = len(shape1)
    channel2 = len(shape2)
    
    if channel1 != channel2:
        if channel1 == 2: image1 = cv2.cvtColor(image1, cv2.COLOR_GRAY2BGR)
        elif channel2 == 2: image2 = cv2.cvtColor(image2, cv2.COLOR_GRAY2BGR)
    
    if H1 != H2 or W1 != W2:
        image2 = cv2.resize(image2, dsize=(H1, W1), interpolation=cv2.INTER_LINEAR)
    
    # -> 이미지의 shape이 같아야 stack 할 수 있음!
    if t_axis:
        concat_image = np.hstack((image1, image2))
    else:
        concat_image = np.vstack((image1, image2))
    
    return concat_image
```

### 📌 def concat_images_horizontally()
```Python
def concat_images_horizontally(*images: np.ndarray):
    width_list = [img.shape[1] for img in images]
    t_width = min(width_list)
    
    import imutils
    
    concat_image = None
    
    for i, img in enumerate(images):
        # -> 차원 수와 높이가 같아야 concatenate 가능
        if img.shape[1] != t_width:
            img = imutils.resize(img, width=t_width)
            
        if len(img.shape) == 2:
            img = cv2.cvtColor(img, cv2.COLOR_GRAY2BGR)
            
        if not i:
            concat_image = img
        else:
            concat_image = np.concatenate((concat_image, img), axis=1)
    
    return concat_image
```