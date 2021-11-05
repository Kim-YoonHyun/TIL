# Word cloud

- 패키지 다운로드 등의 문제로 이 내용은 colab 에서 하는 편이 좋음
- 그대로 복사해서 따라가면 어지간해선 가능

## 기본

### 한글 설정(Colab)

```python
# 한글 폰트 설치
!apt-get install -y fonts-nanum > /dev/null
!fc-cache -fv > /dev/null
!rm -rf ~/.cache/matplotlib > /dev/null
# 런타임 다시 시작
```

> 한글 텍스트 전처리 패키지 선택

```python
!pip install konlpy > /dev/null
```

> 파일 업로드(이미 파일이 존재할 경우 필요없는 코드)

```python
from google.colab import files
uploaded = files.upload()
textfile = list(uploaded.keys())[0]		# 텍스트 파일
with open(textfile) as fp:
    text = fp.read()			
```

```python
uploaded = files.upload()
maskfile = list(uploaded.keys())[0]		# 이미지 파일
```

> 전처리

```python
from konlpy.tag import Okt
okt = Okt()
tokens = okt.nouns(text)		# 명사 추출
tokens[:10]
```

> 영문자, 숫자 제거

```python
import re
new_tokens = []
for token in tokens:
    item = re.sub('[A-Za-z0-9]', '', token)
    if item:
        new_tokens.append(item)
```

> 한글 폰트 사용

```python
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
mpl.rcParams['axes.unicode_minus'] = False
plt.rc('font', family='NanumBarunGothic') 
```

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

> 용어 빈도 파악

```python
gift = nltk.Text(new_tokens, name='여친선물')
plt.figure(figsize=(15,6))
gift.plot(50)
plt.show()
```

```python
stoptext = """
    이곳에 삭제할 단어를 띄어쓰기 식으로 적어서 표현
"""
stop_words = stoptext.split()
new_tokens = [word for word in new_tokens if word not in stop_words]
new_tokens[:10]
```

> 한글자 단어 보고싶은 경우

```python
# import numpy as np
# aaa = np.unique(new_tokens)
# for idx, word in enumerate(aaa):
#   if len(word) == 1:
#     print(word, end=' ')
```

> 임포트

```python
from wordcloud import WordCloud
from PIL import Image
```

```python
data = gift.vocab().most_common(300)
path = '/usr/share/fonts/truetype/nanum/NanumGothic.ttf'
wc = WordCloud(
        font_path=path, relative_scaling=0.2,
        background_color='white'
).generate_from_frequencies(dict(data))
```

> 기본 word cloud

```python
plt.figure(figsize=(12,6))
plt.imshow(wc, interpolation='bilinear')
plt.axis('off')
plt.show()
```

> 그림에 표시할 경우

```python
mask = np.array(Image.open(maskfile))

from wordcloud import ImageColorGenerator
image_colors = ImageColorGenerator(mask)
```

```python
wc = WordCloud(
        font_path=path, relative_scaling=0.2,
        background_color='white', mask=mask,
        min_font_size=1, max_font_size=120
).generate_from_frequencies(dict(data))
```

> 그림에 덧씌운 wordcloud 표시

```python
plt.figure(figsize=(10,10))
plt.imshow(wc.recolor(color_func=image_colors), 
           interpolation='bilinear')
plt.axis('off')
plt.savefig(f'image_name.png')			# 그림 저장
plt.show()
```

