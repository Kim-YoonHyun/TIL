# 1. 개요

[설명 블로그](https://zzsza.github.io/mlops/2021/02/07/python-streamlit-dashboard/)

파이썬 기반 웹 어플리케이션 tool.

ML 프로젝트 등을 배포할 때 유용함.

- 기본적인 사용 순서.
  1. streamlit 설치 (CLI)
  2. streamlit 기반 코드 작성. (python)
  3. streamlit 을 통해 코드 실행 (CLI)
  4. http://\<host network>:8501 로 접속 가능

# 2. CLI 명령어

## 2.1. 설치

```bash
$ pip install streamlit
```

## 2.2. 실행

```bash
$ streamlit run <python file>.py

You can now view your Streamlit app in your browser.
  Local URL: http://localhost:8501
  Network URL: http://<my_network>:8501
```

# 3. python 코드 명령어

## 3.1. 기본 import

```python
import streamlit as st
```

## 3.2. 텍스트 출력

### 3.2.1. title

```python
st.title('This is title')
```

### 3.2.2. header, subheader

```python
st.header('This is header')
st.subheader('This is subheader')
```

### 3.2.3. text

```python
st.text('This is text.')
```

### 3.2.4. write

```python
st.write('This is write')
```

### 3.2.5. code

```python
# one line
st.code('import numpy as np')
```

```python
with st.echo():	# 이하 내용은 전부 코드블럭으로 입력됨.
    import pandas as pd
    df = pd.DataFrame()
```

### 3.2.6. json 

```python
st.json({'name':'민수', 'gender':'male', 'Age':29})
```



## 3.3. widget 생성

### 3.3.1. button

```python
button = st.button('click button')

# if 함수를 통해 버튼 클릭 활성화.
if button:
    st.write("Data Loading..")
```

### 3.3.2. check box

```python
checkbox = st.checkbox('Checkbox Button')

if checkbox:
    st.write('checkbox checked')
```

```python
# 체크된 상태를 default 로 설정
checkbox = st.checkbox('Checkbox Button', value=True)
```

### 3.3.3. radio button

첫 요소가 default 로 선택되어있음.

```python
radio_button = st.radio('radio button', ('a', 'b', 'c'))

if radio_button == 'a':
    st.write('a')
elif radio_button == 'b':
    st.write('b')
elif radio_button == 'c':
    st.write('c')
```

### 3.3.4. select box

```python
selectbox = st.selectbox(
    'select the items', ('a', 'b', 'c')
                        )
st.write(selectbox)
```

### 3.3.5. multi select box

- 결과값이 배열로 나옴.

```python
multi_selectbox = st.multiselect(
    'select the items', ('a', 'b', 'c', 'd')
    )
st.write(multi_selectbox)
```

### 3.3.6. slider

- 결과값은 범위의 튜플 값.
- slider 범위 및 default 범위는 `int` 또는 `float` 로 통일 되어야함.

```python
slider = st.slider('select range', # 제목
                   0.0, 100.0,	   # slider 범위
                   (25.0, 75.0))   # default 범위
st.write(values)
```

### 3.3.7. progress bar

```python
# Add a placeholder
latest_iteration = st.empty()
bar = st.progress(0)

for i in range(100):
    # Update the progress bar with each iteration.
    latest_iteration.text(f'Iteration {i+1}')
    bar.progress(i + 1)
    time.sleep(0.1)
```



## 3.4. 값 입력 받기

### 3.4.1. 텍스트 입력

```python
text = st.text_input('text input', 'value')
```

```python
# 암호화
text_pw = st.text_input('text input', 'value', type='password')
```

### 3.4.2. 텍스트 여러줄 입력

```python
texts = st.text_area('text input', 'value')
```

### 3.4.3. 숫자 입력

```python
number = st.number_input('number input', 1)
```

### 3.4.4. 날짜 및 시간 입력

- datetime 의 튜플 형식으로 넣어야함.

```python
import datetime

# 날짜 입력
date = st.date_input('date', datetime.date(2020, 12, 31))
st.write(date)
```

```python
# 시간 입력
time = st.time_input('time', datetime.time(10, 15))
```

시간의 경우 '시간:분' 의 형태로만 가능하며 분은 15분 단위로만 입력가능.

## 3.5. data 출력

```python
st.table(df)
```

`st.table`은 고정된 데이터 테이블을 볼 수 있음 (스크롤바 등 없이 전체 표시)

```python
st.dataframe(df)
st.dataframe(df.style.highlight_max(axis=0))
```

`st.dataframe` 은 유동적인 테이블 볼 수 있음 (스크롤바 등을 통한 표시)

`.style.highlight_max(axis=0)` 옵션을 통해 최댓값 하이라이트 가능.

예시(`pandas` 필요)

```python
iris = load_iris()
iris_df = pd.DataFrame(iris.data, columns=iris.feature_names)

st.table(iris_df)
st.dataframe(iris_df)
st.dataframe(iris_df.style.highlight_max(axis=0))
```

## 3.6. data 차트 출력

Matplotlib, [Altair](https://altair-viz.github.io/gallery/), Deck.gl, Plot.ly, Bokeh, Graphviz 등을 통해 시각화.

```python
st.line_chart(df)
st.area_chart(df)
st.bar_chart(df)
```

## 3.7. message

### 3.7.1. success

```python
st.success('success')
```

### 3.7.2. error

```python
st.success('error')
```

### 3.7.3. waring

```python
st.warning('warning')
```

### 3.7.4. information

```python
st.info('information')
```

### 3.7.5. exception

```python
st.exception('exception')
```

### 3.7.6. 코드 실행 도중 출력

```python
import time
	
with st.spinner('Wait for it...'):
    for i in range(10):
        time.sleep(1)
        st.write(i + 1)
    
st.success('Done!')
```

## 3.8. 컨텐츠 출력

### 3.8.1. image

```python
from PIL import Image
img = Image.open('cat.jpg')

st.image(img)
```

### 3.8.2. video

```python
video_file = open('video.mp4', 'rb')
video_bytes = video_file.read()

st.video(video_bytes)
```

### 3.8.3. audio

```python
audio_file = open('audio.ogg', 'rb')
audio_bytes = audio_file.read()
	
st.audio(audio_bytes, format='audio/ogg')
```

## 3.9. 사이드바

### 3.9.1. select box sidebar

```python
add_sidebar = st.sidebar.selectbox("Select Box", ("A", "B", "C"))
```

## 3.10. 레이아웃

```python
col1, col2, col3 = st.beta_columns(3)

with col1:
   st.header("A cat")
   st.image("cat.jpg", use_column_width=True)

with col2:
   st.header("Button")
   if st.button("Button!!"):
       st.write("Yes")

with col3:
	st.header("Chart Data")
	chart_data = pd.DataFrame(np.random.randn(50, 3), columns=["a", "b", "c"])
	st.bar_chart(iris_df)
```











## 3.11. markdown 지원

```python
st.markdown('<markdown 문법>')

# 예시.
st.markdown("# This is a Markdown title")
st.markdown("## This is a Markdown header")
st.markdown("### This is a Markdown subheader")
st.markdown("- item 1\n"
            "   - item 1.1\n"
            "   - item 1.2\n"
            "- item 2\n"
            "- item 3")
st.markdown("1. item 1\n"
            "   1. item 1.1\n"
            "   2. item 1.2\n"
            "2. item 2\n"
            "3. item 3")
```

## 3.4. latex

```python
st.latex(r”Y = \alpha + \beta X_i”)
```



# 4. app 배포.

streamlit 기반의 웹 어플리케이션을 배포(Deploy)하는 방법.

1. ~~heroku 를 통한 배포~~  -> 너무 복잡하고 설명이 중구난방이라 도저히 하는 방법을 모르겠음.
2. streamlit 에서 자체 제공하는 streamlit cloud 에서 배포 -> 현재 이 방법으로 진행.

## 4.1. streamlit sharing

이 방법은 기본적으로 github 와 연동하는 방식으로 진행됨.

즉, **github 계정이 있다는 것을 전제**로 진행.

### 4.1.1. 진행 순서

1. streamlit cloud 접속

2. 오른쪽 상단의 Sign in

   ![signin](C:\Users\10-210917\Desktop\public git\TIL\signin.png)

3. Continue with GitHub 를 통해 가입 및 계정 연동 진행

   ![makeaccount](C:\Users\10-210917\Desktop\public git\TIL\makeaccount.png)

4. New app 을 통해 git repo, branch, 실행파일 선택.

   단, 연동되는 git 레파지토리에는 반드시 requirements.txt가 존재해야함.

5. git 레파지토리를 갱신(push) 해줄때마다 자동 배포됨.

### 4.1.2. requirements.txt

실행 파일의 모듈 등의 정보가 들어가 있는 파일.

반드시 연동된 github의 repo에 있어야함.

- cv2 module: opencv-python-headless 추가.
- tensorflow: tensorflow==1.14.0 추가.
