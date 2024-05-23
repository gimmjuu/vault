# OpenCV

## Video FPS

### 모니터 주사율 Hz

- 수직 주파수, 화면 재생율
- 주사율 단위 Hz 헤르츠 : 1초에 몇 개의 화면을 보여주는가를 의미
- 모니터 수직 주파수 사양은 60~360Hz까지 다양, 고주사율 → 고사양

### 컴퓨터 프레임율 FPS

- 새로고침빈도, 화면재생빈도
- FPS == Frame Per Second : 1초에 몇 개의 프레임(화면)을 만들어 내느냐
- 내장 또는 외장 그래픽과 관련된 사양, 고프레임률 → 고사양

** 모니터의 수직 주파수와 그래픽 카드의 새로고침 빈도는 의미 상 동일함 **

1. Hz < FPS
    - 화면 찢어짐 Tearing 테어링 발생 (화면과 화면이 어긋나서 겹쳐 보임)
2. Hz > FPS
    - 화면 끊김 Stuttering 스터터링 발생 (화면이 비연속적으로 끊어져 보임)

* Input lag 인풋렉 : 화면 전송 및 입력 처리 시간 지연 현상

- 모니터 주사율 확인 후 카메라 FPS 변경

```
$ pip install pypiwin32
```

```python
from time import time
import win32api
import win32con
import cv2


def get_refresh_rate() -> int:
    # 현재 모니터 주사율 반환
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
