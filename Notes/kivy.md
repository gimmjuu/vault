---
tags:
  - PYTHON
  - GUI
---
# Kivy

> <span style="font-weight: bold;color: rgb(255,255,255);background: rgba(255,255,0,0.25);">참고</span> [ [Kivy 시작하기](https://streamls.tistory.com/entry/%ED%8C%8C%EC%9D%B4%EC%8D%AC-kivy-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-1-%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83-%ED%8F%AC%ED%95%A8no-kv-%ED%8C%8C%EC%9D%BC#google_vignette) ]

- 크로스 플랫폼 파이썬 라이브러리
    - Windows
    - Mac
    - Linux
    - Android
    - iOS
- 멀티터치 애플리케이션 개발 특화
    - 다양한 터치 스크린 장치에서 작동하는 인터페이스 구현
- 고성능 그래픽
    - OpenGL 기반 2D/3D 그래픽
- 사용자 정의 위젯
    - 다양한 UI 요소 및 사용자 정의 위젯 지원
- 이벤트 드리븐 프로그래밍
    - 비동기식 이벤트 처리
    - 반응형 애플리케이션 구축
- 사용 용이성, 확장 가능한 구조

## 🎇 Kivy App Life Cycle
1. App brought to foreground after process is killed(Android/iOS)
2. Kivy Bootstrap for android/ios
3. Python start, run()
4. build()
5. on_start()
6. Apps functions
7. External app/os or internal function pauses app
8. on_pause()
    - Save your work here.
    - Resume is not guaranteed
    - on_pause()?
9. Resume?
    - on_resume()
    - on_stop()
10. Kivy Window Destroyed
11. Python stop

## 🎇 Install
```bash
python -m pip install --upgrade pip setuptools virtualenv
python -m venv kivy_venv
source kivy_venv/Scripts/activate
pip install kivy
```

### 📌 `uix`
- the section that holds the user interface elements like layouts and widgets

## 🎇 Create an application
- `main.py`
```python
import kivy

kivy.require('2.1.0')  # Replace with your current kivy version!

from kivy.app import App
from kivy.uix.label import Label
from kivy.uix.button import Button

class TestApp(App):  #  defining the Base Class of this Kivy App
    def build(self):  # the function that should initialize and return Root Widget Instance
        # return Button(text='Hello, Kivy!')
        return Label(text='Hello, Kivy!')  # initialize a Label with text ‘Hello World’ and return label instance

if __name__ == "__main__":
    TestApp().run()
``` 

## 🎇 Running the application
```bash
python main.py
```

## 🎇 Customize the application
```python
from kivy.app import App
from kivy.uix.gridlayout import GridLayout  # Import a Gridlayout
from kivy.uix.label import Label
from kivy.uix.textinput import TextInput

class LoginScreen(GridLayout):  # as a Base for a Root Widget
    def __init__(self, **kwargs):# Override the method __init__() so as to add widgets and to define their behavior
        super(LoginScreen, self).__init__(**kwargs)  # not to omit the **kwargs while calling super
        self.cols = 2
        self.add_widget(Label(text='User Name'))
        self.username = TextInput(multiline=False)
        self.add_widget(self.username)
        self.add_widget(Label(text='password'))
        self.password = TextInput(password=True, multiline=False)
        self.add_widget(self.password)

class MyApp(App):
    def build(self):
        return LoginScreen()

if __name__ == "__main__":
    MyApp().run()
```
