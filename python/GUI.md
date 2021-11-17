# GUI 툴킷

tkinter, PyQt, wxPython, PyGui, PySide 등 다양하게 존재

## tkinter

```python
import tkinter as tk
from tkinter import *
```

### root window

#### 윈도우 생성

```python
root_window = tk.Tk()
```

#### 윈도우 제목 설정

```python
root_window.title('title')
```

#### 윈도우 크기 설정

```python
root_window.geometry('500x200')
```

#### 윈도우 실행

```python
root_window.mainloop()		# 가장 마지막 부분에 적음
```

#### 윈도우 끄기

```python
root_window.destroy()
```

### 위젯 생성

모든 위젯은 기본적으로
`bg='red', fg='blue', width=10, height=3`
의 옵션을 가지고 있음.

단, entry는 높이 설정이 불가능하여 height가 없는 것처럼 특정 위젯은 없는 경우도 있음.

#### label 

```python
label = tk.Label(root_window, text='test app')
```

#### message

현재로썬 Label 과의 차이점 불명

```python
msg = tf.Message(root_window, text='msee')
```

#### entry

한줄 문자열 입력

````python
entry = tk.Entry(root_window)
````

#### text

여러줄 문자열 입력

````python
text = tk.Text(root_window)
````

#### button

```python
button = tk.Button(root_window, text='버튼')
```

#### Checkbutton

```python
ch1 = tf.Checkbutton(root_window, text='check1')
```

#### list box

```python
list_box = tk.Listbox(root_window, exportselection=False)	
# exportselection: 
# 포커스를 잃더라도 선택된 항목이 그대로 남아있도록 하기 위한 옵션
```

### 위젯의 값

#### 값 입력

```python
<위젯>.insert(<위치>, 'text')
entry.insert(0, 'string')		# 0 부터 시작. 정수값 가능
text.insert(1.0, 'string')		# 1.0 부터 시작. 실수값만 가능.
list_box.insert(0, 'text')		# 0위치에 text 리스트박스 생성
list_box.insert(tk.END, 'text')	# 마지막에 text 리스트박스 생성
```

#### 입력 값 가져오기

```python
entry.get()
list_box.curselection()		# 현재 선택된 리스트 박스의 인덱스를 튜플 형태로 리턴
```

#### 값 삭제

```python
<위젯>. delet(<시작>, <끝>)
entry.delete(0, 1)			# 0 ~ 1 까지의 값 삭제
text.delete(1.0, tk.END)	# 전부 삭제
list_box.delete(0, 2)		# 0 ~ 2 위치 삭제
```

### 이미지 넣기

PhotoImage로 불러와서 위젯을 통해 삽입

```python
img = tk.PhotoImage(file=<path>)
img_wg = tk.Label(root_window, image=img)
img_wg = tk.Button(root_window, image=img)
```

### 위젯 배치

#### place

위젯 위치를 절대 좌표로 정하는 방식. 위젯의 크기는 변하지 않는다.

```python
<위젯>.place(0, 1)
```

#### pack

부모위젯에 패킹하여 불필요한 공간을 없애는 방색.

```python
<위젯>.pack()
```

#### grid

객체 하나하나가 전부 하나의 row 이자 col 을 생성함.
row: 객체의 행 위치
col: 객체의 열 위치
columnspan: 열방향 객체 범위 확장
ew: 동서 방향 길이 맞춤
ns: 남북 방향 길이 맞춤
으로 객체의 위치&크기 지정 가능

```python
<위젯>.grid(row=1, column=0)	# 위젯 생성시 크기를 지정했을 경우 사용
<위젯>.grid(row=0, column=3, columnspan=3, sticky='ew')
<위젯>.grid(row=4, column=2, columnspan=3, sticky='ns')
```

### 바인딩

#### 클릭시 이벤트

```python
def button_click(event):
    print('위젯이 클릭되었습니다.')
<위젯>.bind('<Button-1>', button_click)
```

#### 리스트 박스 선택

```python
def event_for_list_box(event):	# 예시 함수
    print('event')

list_box.bind('<<ListboxSelect>>', event_for_list_box)
```

#### 커서가 위에 올라갈 경우 이벤트

```python
def mouse_in(event):
    print('마우스가 위젯 위에 있습니다.')
<위젯>.bind('<Enter>', mouse_in)
```

#### 커서가 밖으로 나올 경우 이벤트

```python
def mouse_out(event):
    print('마우스가 위젯 밖으로 나왔습니다.')
<위젯>.bind('<Leave>', mouse_out)
```

#### ESC 단축키 설정

```python
def closer(event):
	pass 
<위젯>.bind('<Escape>', closer)
<위젯>.bind('<q>', closer)
```

#### 엔터키 단축키 설정

```python
<위젯>.bind('<Return>', going)
<위젯>.bind('<r>', going)
```

### 탭 생성

```python
import tkinter.ttk as ttk

notebook = ttk.Notebook(root_window, width=800, height=500)
tab1 = tk.Frame(root_window)		# 탭 추가
tab2 = tk.Frame(root_window)

notebook.add(tab1, text="TAB1")		# 탭 이름
notebook.add(tab2, text="TAB2")

label1=tk.Label(tab1, text="TAB1")
label2=tk.Label(tab2, text="TAB2")
```

이후 위젯을 root_window 가 아닌 tab1 또는 tab2에 생성하면 그 탭에 생성됨

### 파일에 접근(filedialog)

#### 불러오기

```python
import tkinter.filedialog as fd

filedialog.asksaveasfilename()
filedialog.asksaveasfile()
filedialog.askopenfilename()
filedialog.askopenfile()
filedialog.askdirectory()
filedialog.askopenfilenames()
filedialog.askopenfiles()
```

### 스크롤 바

#### pack 방식

#### grid 방식

```python
<적용할 위젯 생성>

# x축은 현재 잘 되지 않음
#scrollbar = tf.Scrollbar(root_window, orient='horizontal', 
#                         command=<적용할 위젯>.xview)
#<적용할 위젯>.['xscrollcommand'] = scrollbar.set
#scrollbar.grid(row=0, column=3, sticky='ew')

# y 축
scrollbar = tf.Scrollbar(root_window, orient='vertical', 
                         command=<적용할 위젯>.yview)
<적용할 위젯>.['yscrollcommand'] = scrollbar.set
scrollbar.grid(row=0, column=3, sticky='ns')
```

탭을 생성할 경우 root_window 대신 tab 이름을 적어야한다.

스크롤바는 기본적으로 위젯을 생성한 후 그 위젯에 적용하는 방식이 가장 무난함.

### 오류 메세지

```python
tk.messagebox.showerror('오류', '오류가 발생했습니다.')
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

