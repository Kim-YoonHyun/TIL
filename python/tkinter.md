# 1. 개요

파이썬에서 GUI를 다루는 모듈 중 하나.
tkinter 외에 PyQt, wxPython, PyGui, PySide 등 다양하게 존재

# 2. 명령어

```python
import tkinter as tk
from tkinter import *
```

## 2.1. root window

### 2.1.1. 윈도우 생성

```python
root_window = tk.Tk()
```

### 2.1.2. 윈도우 제목 설정

```python
root_window.title('title')
```

### 2.1.3. 윈도우 크기 설정

```python
root_window.geometry('500x200+100+100')	# 공백 있으면 안됨
# 500 x 200 : 너비 X 높이 크기 설정
# 100+100 : 메인화면이 출력할 시작 좌표. 100, 50 만큼 우측과 아래로 이동함.  
```

```python
root_window.resizable(width=False, height=False)	# 창 크기 고정
```

### 2.1.4. 윈도우 실행

```python
root_window.mainloop()		# 가장 마지막 부분에 적음
```

### 2.1.5. 윈도우 끄기

```python
root_window.destroy()
```

### 2.1.6. root 윈도우에 추가 윈도우 생성

```python
new_window = tk.Toplevel(root_window)
# new_window.mainloop()는 할 필요 없음
```

### 2.1.7. root 윈도우에 메뉴바 추가.

```python
menubar = tk.Menu(root_window)

filemenu1 = tk.Menu(menubar, tearoff=0)
filemenu1.add_command(label='Save1')
filemenu1.add_command(label='Save2')
menubar.add_cascade(label='File1', menu=filemenu1)

filemenu2 = tk.Menu(menubar, tearoff=0)
filemenu2.add_command(label='quit1')
filemenu2.add_command(label='quit2')
menubar.add_cascade(label='File2', menu=filemenu2)

root_window.config(menu=menubar)
```

### 2.1.8. 메뉴바에 단축키 설정

[2.11. 메뉴바 단축키 설정](#2.11. 메뉴바 단축키 설정) 참고.



## 2.2. 위젯 생성

### 2.2.1. 공통 옵션

| 이름    | 의미                                  | 기본값           | 속성                                       |
| ------- | ------------------------------------- | ---------------- | ------------------------------------------ |
| width   | 너비                                  | 0                | 상수                                       |
| height  | 높이                                  | 0                | 상수                                       |
| relief  | 테두리모양                            | flat             | flat, groove, raised, ridge, solid, sunken |
| bd      | 테두리두께                            | 2                | 상수                                       |
| bg      | 배경색상                              | SystemButtonFace | color                                      |
| fg      | 문자열 색상                           | SystenButtonFace | color                                      |
| padx    | 라벨의 테두리와<br />내용의 세로 여백 | 1                | 상수                                       |
| pady    | 라벨의 테두리와<br />내용의 세로 여백 | 1                | 상수                                       |
| justify | 정렬                                  | center           | center, right, left                        |
| state   | 위젯 활성화 여부                      | 'normal'         | string ('normal' or 'disable')             |

단, entry 처럼 높이 설정이 불가능한 경우도 있음

entry, text 와 같이 string을 입력 가능한 위젯의 경우 width 및 height의 측정 단위가 글자 기준임.
이에, sticky와는 상관없이 크기를 정하기 않으면 항상 크게 설정됨.

string이 입력 가능하지 않은 widget의 크기는 윈도우 픽셀에 의해 결정되며, sticky가 유효함.

즉, 기본적으로 entry, text로 크기를 조정하고 그 외의 widget은 크기조절 또는 sticky로 따라가는것이 나음.

### 2.2.2. label 

```python
label = tk.Label(root_window, text='test app')
```

### 2.2.3. message

현재로썬 Label 과의 차이점 불명

```python
msg = tf.Message(root_window, text='msee')
```

### 2.2.4. entry

한줄 문자열 입력

````python
entry = tk.Entry(root_window)
````

### 2.2.5. text

여러줄 문자열 입력

````python
text = tk.Text(root_window)
````

### 2.2.6. button

```python
button = tk.Button(root_window, text='버튼')
```

### 2.2.7. Checkbutton

```python
ch1 = tf.Checkbutton(root_window, text='check1')
```

### 2.2.8. list box

```python
list_box = tk.Listbox(root_window, exportselection=False)	
# exportselection: 
# 포커스를 잃더라도 선택된 항목이 그대로 남아있도록 하기 위한 옵션
```

### 2.2.9. Canvas

```python
canvas = tk.Canvas(root_window)
# width : 너비. default=378
# height: 높이. default=265
# relidf: 테두리 모양. flat,groove, raised, ridge, solid, sunken
# bd: 테두리 두께. default=0
# bg: 배경색. default=SystemButtonFace
# offset: 오프셋 설정. x, y, n, e, w, s, ne, nw, se, sw
```

### 2.2.10. scrollbar

#### 2.2.10.1. pack 방식

#### 2.2.10.2. grid 방식

```python
<적용할 위젯 생성>

# x축은 현재 잘 되지 않음
#scrollbar = tk.Scrollbar(root_window, orient='horizontal', 
#                         command=<적용할 위젯>.xview)
#<적용할 위젯>.['xscrollcommand'] = scrollbar.set
#scrollbar.grid(row=0, column=3, sticky='ew')

# y 축
scrollbar = tk.Scrollbar(root_window, orient='vertical', 
                         command=<적용할 위젯>.yview)
<적용할 위젯>['yscrollcommand'] = scrollbar.set
scrollbar.grid(row=0, column=3, sticky='ns')
```

탭을 생성할 경우 root_window 대신 tab 이름을 적어야한다.

스크롤바는 기본적으로 위젯을 생성한 후 그 위젯에 적용하는 방식이 가장 무난함.

#### 2.2.10.3. canvas 에 scrollbar 적용

캔버스에서 스크롤 영역을 지정해 주는 명령어.
이게 없으면 스크롤 바가 생겨도 움직이지 않음.

```python
canvas.config(scrollregion=(x1, y1, x2, y2))   # 영역 지정
canvas.config(scrollregion=canvas.bbox('all')) # 캔버스 전체에 적용
```



### 2.2.11. message box

```python
tk.messagebox.showerror('오류', '오류가 발생했습니다.')
tk.messagebox.askyesno('선택창', '선택하시겠습니까?')	# 확인 시 True 리턴
tk.messagebox.showwarning('경고', '경고입니다.')
```

## 2.3. 위젯 배치

### 2.3.1. place

위젯 위치를 절대 좌표로 정하는 방식. 위젯의 크기는 변하지 않는다.

```python
<위젯>.place(0, 1)
```

### 2.3.2. pack

부모위젯에 패킹하여 불필요한 공간을 없애는 방색.

```python
<위젯>.pack()
```

### 2.3.3. grid

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

## 2.4. 위젯 값 입력

### 2.4.1. entry

```python
entry.insert(0, 'string')		# 0 부터 시작. 정수값 가능
```

### 2.4.2. text

```python 
text.insert(1.0, 'string')		# 1.0 부터 시작. 실수값만 가능.
# 1.0 : 1번째 줄 0번째 단어.
```

#### tag_add

```python
text.tag_add('tagging', 1.0, 2.2)	# 1.0 부터 2.2 까지 태그 지정
```

#### tag_config

```python
text.tag_config('tagging', background='blue', foreground='red')
# background: 글자 배경색
# foreground: 글자 색
```

### 2.4.3. listbox

```python
listbox.insert(0, 'text')		# 0위치에 text 리스트박스 생성
listbox.insert(tk.END, 'text')	# 마지막에 text 리스트박스 생성
```

### 2.4.5. canvas(그리기)

```python
canvas.create_line(x1, y1, x2, y2, ..., option)	# 좌표에서 좌표 연결 선
canvas.create_rectangle(x1, y1, x2, y2, option)	# 좌상단에서 우하단의 사각형
canvas.create_ploygon(x1, y1, x2, y2, ..., option)	# 좌표에서 좌표 연결 다각형
canvas.create_oval(x1, y1, x2, y2, option)	# 좌표에서 좌표 반지름의 원
canvas.create_arc(x1, y1, x2, y2, start, extent, option)	# 좌표에서 좌표 반지름, start에서 extent 각도의 호
canvas.create_image(x, y, image, option)	# 좌표 위치 이미지 생성
# 옵션
# fill : 색 채워넣기
# outline: 두께 색상
# width: 두께
# anchor: 위치지정
# dash: (대쉬길이px, 빈칸길이px)  ex(5, 1)
```

### 2.4.6. checkbutton (체크 유무)

```python
def ischecked():
    if check_status.get() == 1:
        print('체크되었습니다.')
    elif check_status.get() == 0:
        print('해제되었습니다.')
    else:
        tk.messagebox.showerror('에러', '에러발생')

check_status = tk.IntVar()
# check_status = tk.IntVar(value=1)   # 디폴트 상태를 체크된 상태로 할 경우

ch1.config(variable=check_status	# 체크 값 설정할 int 변수.
          onvalue=1,	# 1 일때 체크된 것으로 설정
          offvalue=0,	# 0 일때 체크 해제된 것으로 설정
          command=ischecked)
```

## 2.5. 위젯 값 가져오기

### 2.5.1. entry

```python
entry.get()
```

### 2.5.2. text

```python
text.get(1.0, tk.END)
```

### 2.5.3. listbox

```python
list_box.curselection()		# 현재 선택된 리스트 박스의 인덱스를 튜플 형태로 리턴
list_box.get(0, tk.END)		# 리스트박스의 항목을 튜플 형태로 리턴
```

## 2.6. 위젯 값 삭제

### 2.6.1. entry

```python
entry.delete(0, 1)			# 0 ~ 1 까지의 값 삭제
```

### 2.6.2. text

```python
text.delete(1.0, tk.END)	# 전부 삭제
```

### 2.6.3. listbox

```python
list_box.delete(0, 2)		# 0 ~ 2 위치 삭제
```

## 2.7. 위젯 수정

```python
<위젯>.config(option)
```

## 2.8. 위젯에 이미지 넣기

PhotoImage로 불러와서 위젯을 통해 삽입

```python
img = tk.PhotoImage(file=<path>)
img_wg = tk.Label(root_window, image=img)
img_wg = tk.Button(root_window, image=img)
```

## 2.9. command

### 2.9.1. button command

```python
def text_def():
    print(10)
button = tk.Button(root_window, text='버튼', command=test_def)

# lambda 를 통한 인수 적용
def test_def(num):
    print(num)
button = tk.Button(root_window, text='버튼', command=lambda: test_def(10))
```

## 2.10. bind

### 2.10.1. 마우스 관련

https://076923.github.io/posts/Python-tkinter-33/

```python
def exam_def(event):
    print(123)
    
<위젯>.bind('<Button-1>', exam_def)	# 왼쪽 클릭
<위젯>.bind('<Button-2>', exam_def)	# 휠 클릭
<위젯>.bind('<Button-3>', exam_def)	# 오른쪽 클릭
<위젯>.bind('<Button-4>', exam_def)	# 스크롤 업
<위젯>.bind('<Button-5>', exam_def)	# 스크롤 다운
<위젯>.bind('<Enter>', exam_def)		# 마우스가 위젯 안으로 들어감
<위젯>.bind('<Leave>', exam_def)		# 마우스가 위젯 밖으로 나감
```

### 2.10.2. listbox select

```python
def event_for_list_box(event):
    print('event')

list_box.bind('<<ListboxSelect>>', event_for_list_box)
```

### 2.10.3. 키보드 관련

```python
def closer(event):
	pass 
<위젯>.bind('<Key>', closer)		# 특정 키
<위젯>.bind('<Escape>', closer)		# ESC 키
<위젯>.bind('<q>', closer)
<위젯>.bind('<Return>', closer)		# Enter 키
<위젯>.bind('<BackSpace>', closer)	# Backspace 키
<위젯>.bind('<Up>', closer)		# 위쪽 방향키
<위젯>.bind('<Down>', closer)		# 아래쪽 방향카
<위젯>.bind('<Right>', closer)	# 오른쪽 방향키
<위젯>.bind('<Left>', closer)		# 왼쪽 방향키
```

### 2.10.4. lambda 적용

```python
def asd(a, b, event=None):
    print(a, b)
# listbox bind
<위젯>.bind('<이벤트>', lambda event: asd(1, 2)) 
```

event 부분은 lambda를 쓰기위한 일종의 장치일뿐 의미없음.

아마 정식 방법은 아니겠지만 문제없이 작동함.

## 2.11. 메뉴바 단축키 설정

```python
def save(event=None):
    print('저장되었습니다.')

menubar = tk.Menu(root_window)

filemenu = tk.Menu(menubar, tearoff=0)
filemenu.add_command(label='Save', command=save, accelerator='Ctrl+S')
filemenu.bind_all('<Control-s>', save)

menubar.add_cascade(label='File', menu=filemenu)

root_window.config(menu=menubar)
```

## 2.12. 탭 생성

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

## 2.13. 파일에 접근(filedialog)

### 2.13.1. 불러오기

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

