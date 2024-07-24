run.py
```
from ultralytics import YOLO
import cv2
from dateutil import tz
from datetime import datetime
import os
import json
import redis
from flask import Flask, Response

app = Flask(__name__)

panel_model = YOLO("../solar_panel/solar_panel.pt")
panel_path = "rtsp://admin:gw[bg]imr22@115.143.44.94:54554/profile2/media.smp"

cctv_model = YOLO("../solar_panel/yolov8s.pt")
# cctv_path = "rtsp://admin:gw[bg]imr22@115.143.44.94:54554/profile2/media.smp"
# cctv_path = "rtsp://admin:gw[bg]imr22@182.226.173.82:54554/profile2/media.smp"
cctv_path = "rtsp://admin:gw[bg]imr22@124.58.148.62:54554/profile4/media.smp"

# redis 서버 실행
'''
redis-server --protected-mode no
nohup redis-server --protected-mode no &
'''

# Redis 서버에 연결
r = redis.Redis(host='0.0.0.0', port=6379, decode_responses=True, db=0)


# 데이터 스트림에 메시지 추가
def produce_messages(stream_name, messages):
    print(f"Message sent: {messages}")
    json_data = json.dumps(messages)
    r.xadd(stream_name, {'messages':json_data})
    # r.publish(stream_name, messages)


def save_img(frame):
    utc_zone = tz.gettz('UTC')
    seoul_zone = tz.gettz('Asia/Seoul')
    utc = datetime.now()
    time = utc.replace(tzinfo=utc_zone).astimezone(seoul_zone)
    timestr = time.strftime("%Y%m%d-%H%M%S")
    savedate = time.strftime("%m%d")
    save_dir = f'./images/{savedate}'
    os.makedirs(f'{save_dir}', exist_ok = True)
    
    img_copy = frame.copy()
    

def detector(frame, model):
    # results = model(frame, conf=0.25, device = 'cpu')
    results = model(frame, conf=0.35, device = 'cpu', classes=0, workers=10)
        
    if not results or len(results) == 0:
        return frame

    data = list()
    
    for _, box in enumerate(results[0].boxes):
        xmin, ymin, xmax, ymax = map(int, box.xyxy[0])
        label = model.names[int(box.cls)]
        data.append(
            {"label": label, 'coordinates': [xmin, ymin, xmax, ymax]}
        )
        cv2.rectangle(frame, (xmin, ymin), (xmax, ymax), (0, 255, 255), 1)
        cv2.putText(frame, label, (xmin, ymin), cv2.FONT_ITALIC, 1, (0, 255, 255), 1)
    
    if data:
        produce_messages("coordinates", data)
    
    return frame


def generate_frames(videoPath, model):
    """카메라 실시간 송출을 위한 mjpeg 생성 함수"""
    cap = cv2.VideoCapture(videoPath)
    
    while True:
        _, img = cap.read()
        img = detector(img, model)
        _, frame = cv2.imencode(".jpg", img)
        
        if frame is None:
            continue
        
        yield (
            b"--frame\r\nContent-Type: image/jpeg\r\n\r\n" + frame.tobytes() + b"\r\n"
        )


@app.route("/stream/panel")
def stream_panel():
    return Response(
        generate_frames(panel_path, panel_model),
        mimetype=("multipart/x-mixed-replace; boundary=frame")
    )


@app.route("/stream/cctv")
def stream_cctv():
    return Response(
        generate_frames(cctv_path, cctv_model),
        mimetype=("multipart/x-mixed-replace; boundary=frame")
    )


@app.route("/")
def index():
    return """
    <body style="background: black;">
        <div style="margin: 0px; padding: 0px;">
            <img src="/stream/cctv" width=860px/>
        </div>
    </body>
    """


def main() -> None:
    print("[server run]")
    app.run(host='0.0.0.0', port=6060, threaded=True)


if __name__ == '__main__':
    main()

```

- consumer.py
```python
import redis
import time

# Redis 서버에 연결
# r = redis.Redis(host='192.168.0.148', port=6378, db=0)
r = redis.Redis(host='192.168.0.200', port=6380, db=0)

# 스트림으로부터 메시지를 읽음
def consume_messages(stream_name, group_name, consumer_name):
    # 소비자 그룹이 존재하지 않으면 생성
    try:
        r.xgroup_create(stream_name, group_name, id='0', mkstream=True)
    except redis.exceptions.ResponseError as e:
        if "BUSYGROUP Consumer Group name already exists" not in str(e):
            raise

    while True:
        # 스트림에서 메시지를 읽음
        messages = r.xreadgroup(group_name, consumer_name, {stream_name: '>'}, count=1, block=5000)
        if messages:
            for stream, message_list in messages:
                for message_id, message in message_list:
                    # print(f"Received: {message},", f"[get] {time.strftime('%M:%S', time.localtime())}")
                    print(f"{message_id}/ Received: {message}")
                    # 메시지 처리 후 ack 전송
                    r.xack(stream_name, group_name, message_id)

# 'coordinates' 스트림의 'mygroup' 그룹에서 메시지 소비
consume_messages('coordinates', 'mygroup', 'consumer1')

# ------ redis subscribe test (publish pair)
# r = redis.Redis(host='192.168.0.148', port=6379, decode_responses=True)

# r = redis.Redis(host='0.0.0.0', port=6378, db=0)
# r = redis.Redis(host='127.0.0.1', port=6378, db=0)
# r = redis.Redis(host='172.17.0.2', port=6378, db=0)

# -- subscribe
# s = r.pubsub()
# s.subscribe('coordinates')
# while True :
#     res = s.get_message()
#     if res is not None :
#         print(res)

#     time.sleep(0.5)
```