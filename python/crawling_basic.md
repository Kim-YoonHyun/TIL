# 1. 개요

## 1.1. 기본적인 흐름

1. 정적 웹페이지 제어(request)인지 동적 웹페이지 제어(selenium)인지 선택

   - 정적 웹페이지(request)

     ```python
     import requests
     url = '<url>'
     req = requests.get(url) # url을 받아옴
     html = req.text
     ```

   - 동적 웹페이지(selenium)

     ```python
     from selenium import webdriver
     driver = webdriver.Chrome('path/chromedriver.exe')
     url = '<url>'
     driver.get(url)
     html = driver.page_source
     ```

     - 웹 페이지 직접제어(클릭, 닫기버튼 누르기 , 입력 등)가 필요할 때

       ```python
       temp = driver.find_element_by_css_selector('class, id 등')	# 하나 찾기
       temp = driver.find_elements_by_css_selector('class, id 등')	  # 복수개 찾기
       temp.click()	# 클릭
       temp.clear()	# 비우기
       temp.send_keys('<단어>')	# 단어 입력 
       temp.submit()	# 로그인
       # 등의 다양한 명령어 존재
       ```

2. html 스크립트를 통해 다양한 정보 크롤링(BeautifulSoup)

   ```python
   from bs4 import BeautifulSoup
   soup = BeautifulSoup(html, 'html.parser')
   ```

   ```python
   soup.find()
   soup.find_all()
   soup.select()
   soup.select_one()
   ```

대개 위의 순서로 크롤링이 진행됨

```python
# url에 한글로 적기
from urllib.parse import quote
url = f'{base_url}/search?keywords={quote("양재역")}'
```

# 2. 명령어

## 2.1. BeautifulSoup

- html 을 통해 다양한 정보를 불러옴

### 2.1.1. 기본 세팅

```python
from bs4 import BeautifulSoup
```

```python
with open('<html 파일>') as file:
    soup = BeautifulSoup(file, 'html.parser')
```

### 2.1.2. 예시 html

```html
<!DOCTYPE html>

<html lang="en">
    <head>
        <meta charset="utf-8"/>
        <meta content="width=device-width, initial-scale=1.0" name="viewport"/>
        <title>Web Crawling Example</title>
    </head>
    <body>
        <div>
            <p>a</p>
            <p>b</p>
            <p>c</p>
        </div>
        <div class="ex_class sample">
            <p>1</p>
            <p>2</p>
            <p>3</p>
        </div>
        <div id="ex_id">
            <p>X</p>
            <p>Y</p>
            <p>Z</p>
        </div>
        <h1>This is a heading.</h1>
        <p>This is a paragraph.</p>
        <p>This is another paragraph.</p>
        <a class="a sample" href="www.naver.com">Naver</a>
    </body>
</html>
```

```python
soup.find('div')		# 최초(가장 위쪽)의 한 개만 찾음
soup.find_all('div')	# 전부 찾음
```

### 2.1.3. CSS Selector

```python
# 전부
soup.select('#ex_id')	# id를 찾을 땐 #을 붙임
soup.select('.sample') 	# class를 찾을 땐 . 을 붙임

# 하나만
soup.select_one('div#ex_id')			
soup.select_one('div.ex_class.sample')		# 띄어쓰기는 .으로
soup.select_one('a.a.sample').get_text()	# 내용을 text로 가져오기
soup.select_one('a.a.sample').string		# 내용을 text로 가져오기(위하고 다름)
soup.select_one('a.a.sample')['herf']		# 속성 값 가져오기
```

> 예시

```python
# id="ex_id" 인 div에서 p 내용물 가져오기
ex_id_div = soup.select_one('div#ex_id')
all_ps = ex_id_div.select('p')
for p in all_ps:
  print(p.string, p.get_text())
```

## 2.2. requests

- url 을 통해 html 을 불러옴 --> 이후 BeautifulSoup를 통해 진행

### 2.2.1. 기본 세팅

```python
import requests
```

```python
url = 'https://www.genie.co.kr/chart/top200'	# url 적기. 예시: genie top200
header = {'User-Agent':'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36'}	# 이게 없으면 에러날때가 있음
req = requests.get(url, headers=header)		# url을 받아옴
html = req.text		# html 을 받아옴
```

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'html.parser')
```

### 2.2.2. 예시 html

```html
...
<table class="list-wrap">		## 1
    <caption>곡 리스트</caption>
    <thead>...</thead>
    <tbody>
    	<tr class="list" songid="93717294">		## 2, ## 3
        	<td class="check">...</td>
        	<td class="number">		## 4
            	"1 " &&&	## 4
            	<span class="rank">...</span>
            </td>
            <td>...</td>
            <td class="link">...</td>
            <td class="info">...</td>
            <td class="more">...</td>
        </tr>
    	<tr class="list" songid="93300145">...</tr>		## 2
        <tr class="list" songid="94020692">...</tr>		## 2
        <tr class="list" songid="92964769">...</tr>		## 2
    </tbody>
</table>>
...
```

```python
# &&& 찾기
table = soup.select_one('table.list-wrap')		## 1: table 의 list-wrap 클래스
trs = table.select('tr.list')					## 2: table 의 모든 tr의 list 클래스 
tr = trs[0]										## 3: 모든 tr 중 첫번째 tr
rank = tr.select_one('td.number').get_text()	## 4: 첫 번째 tr 에서 td 의 number 클래스 내용을 text로 불러옴
```

- 다른 데이터도 같은 매커니즘으로 불러옴

```python
# 첫 번째 tr에서 info 클래스 내부의 title 클래스 내용을 text로 불러와서 strip
title = tr.select_one('.info').select_one('.title').get_text().strip()

# 첫 번째 tr에서 info 클래스 내부의 artist 클래스 내용을 text로 불러와서 strip
artist = tr.select_one('.info').select_one('.artist').get_text().strip()

# 첫 번째 tr에서 info 클래스 내부의 albumtitle 클래스 내용을 text로 불러와서 strip
album = tr.select_one('.info').select_one('.albumtitle').get_text().strip()
```

## 2.3. Selenium

- 웹 어플리케이션을 테스트 하기 위한 라이브러리
- webdriver를 통해 **웹 브라우저를 실행**하고 출력된 화면을 결과로 받는다.
- **동적 웹페이지**에 대한 정보 수집 가능 --> 이후 BeautifulSoup으로 진행

### 2.3.1. 기본 세팅

```python
from selenium import webdriver
```

```python
driver = webdriver.Chrome('chromedriver.exe')
url = 'https://youtube-rank.com/board/bbs/board.php?bo_table=youtube&page=1'
driver.get(url)
```

```python
# 화면 없이 실행 할 경우
# options = webdriver.ChromeOptions()
# options.add_argument('--headless')   # 화면없이 실행
# options.add_argument('--no-sandbox')
# options.add_argument("--single-process")
# options.add_argument("--disable-dev-shm-usage")
# driver = webdriver.Chrome('chromedriver', options=options)
```

### 2.3.2. 요소 불러오기

```python
driver.find_element_by_< >('')  # 하나의 요소
driver.find_elements_by_< >('')  # 복수의 요소
# < > 에 가능한 명령어: 
# name, 
# tag_name, 
# id, 
# class_name, 
# css_selector, : soup 의 css selector 와 같은 방식으로 불러옴
# link_text, 
# partial_link_text, 
# xpath
```

### 2.3.3. 텍스트 값 가져오기

```python
driver.find_element_by_< >.text
```

### 2.3.3. 속성값 가져오기

```python
driver.find_element_by_< >.get_attribute('<속성이름>')  # ex:herf
```

### 2.3.4. 예시

````python
# 아이디 입력 예시
input_email = driver.find_element_by_css_selector('._2hvTZ.pexuQ.zyHYP')[0]
input_email.clear()		# 비우기
input_email.send_keys(email)	# 이메일(아이디 입력)

# 비밀번호 입력 예시
input_pw = driver.find_element_by_css_selector('._2hvTZ.pexuQ.zyHYP')[1]
input_pw.clear()
input_pw.send_keys(pwd)		# 비밀번호 입력
input_pw.submit()		# 로그인하기

# 스크롤 
driver.execute_script("window.scrollTo(0, y)") # y 까지 내림
# 끝까지 내릴 경우는 y 값에 document.body.scrollHeight
driver.execute_script("return document.body.scrollHeight")  # 다시 높이 가져옴

# 뒤로가기
driver.back()
````

> html 불러오기 --> 이후 BeautifulSoup로 진행

```python
html = driver.page_source
soup = BeautifulSoup(html, 'html.parser')

driver.close()		# 모든 제어가 끝난후 닫아줘야함
```

















