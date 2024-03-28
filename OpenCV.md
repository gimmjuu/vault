# OpenCV

## FPS / Hz

- 모니터 주사율 확인 후 카메라 FPS 변경

'''Python3
from time import time
import win32api
import win32con
import cv2

\# $ pip install pypiwin32


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

'''