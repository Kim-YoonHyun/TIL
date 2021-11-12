# GUI 툴킷

tkinter, PyQt, wxPython, PyGui, PySide 등 다양하게 존재

## tkinter

### import

```python
import tkinter
```

### root 윈도우 생성

```python
root_window = tkinter.Tk()
```

### root 윈도우 제목 설정

```python
root_window.title('title')
```

### 객체 생성

#### label

```python
label = tkinter.Label(root_window, text='test app')
```

#### list box

```python
list_box = tkinter.Listbox(root_window, exportselection=False)	# exportselection: 포커스를 잃더라도 선택된 항목이 그대로 남아있도록 하기 위한 옵션
list_box.insert(0, 'text')				# 0위치에 text 리스트박스 생성
list_box.insert(tkinter.END, 'text')	# 마지막에 text 리스트박스 생성
list_box.delete(0, 2)					# 0 ~ 2 위치 삭제
```

#### entry

한줄 문자열 입력

````python
entry = tkinter.Entry(root_window)
entry.insert(0, 'string')
````

#### text

여러줄 문자열 입력

````python
text = tkinter.Text(root_window)
text.insert(1.0, 'string')		# 1.0 부터 시작. 실수값만 가능.
````

#### button

```python
button = tkinter.Button(root_window, text='버튼')
```

### 윈도우에 배치

pack 또는 grid 둘중 하나만 적용 가능

#### 단순배치

기본적으로 명령어 순서대로 행배치, 가운데 열배치가 된다.

```python
<객체>.pack()
```

#### 그리드 배치

객체 하나하나가 전부 하나의 row 이자 col 을 생성함.
row: 객체의 행 위치
col: 객체의 열 위치
columnspan: 열방향 객체 범위 확장
ew: 동서 방향 길이 맞춤
ns: 남북 방향 길이 맞춤
으로 객체의 위치&크기 지정 가능

```python
<객체>.grid(row=1, column=0)
<객체>.grid(row=0, column=3, columnspan=3, sticky='ew')
<객체>.grid(row=4, column=2, columnspan=3, sticky='ns')
```

### 동작 생성

```python
<객체>.bind('<이벤트명령어>', 함수)
```

#### 리스트 박스 선택시 동작

```python
def event_for_list_box(event):	# 예시 함수
    print('event')
    
list_box.bind('<<ListboxSelect>>', event_for_list_box)
```

#### 버튼 클릭시 동작

```python
def button_click(event):
    print('버튼이 클릭되었습니다.')
    
button.bind('<Button-1>', button_click)
```













# PyQt5

```python
import sys
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *
from PyQt5.QtCore import *

class MyApp(QMainWindow):
    def __init__(self):
        super().__init__()
        self.initUI()
        
    def initUI(self):
        <code>

if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```

