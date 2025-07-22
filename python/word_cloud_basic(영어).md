# Word cloud

- 패키지 다운로드 등의 문제로 이 내용은 colab 에서 하는 편이 좋음
- 그대로 복사해서 따라가면 어지간해선 가능

## 기본

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
```

```python
from PIL import Image
import numpy as np
```

> 텍스트 파일 업로드(상황에 따라 필요없음)

```python
from google.colab import files
uploaded = files.upload()
textfile = list(uploaded.keys())[0]
with open(textfile) as fp:
    text = fp.read()
```

> 이미지 파일 업로드

```python
uploaded = files.upload()
maskfile = list(uploaded.keys())[0]
```

```python
from nltk.corpus import stopwords
stop_words = set(stopwords.words('english'))
stop_words.add('said')
```

```python
wc = WordCloud(background_color='white',
               max_words=1000, stopwords=stop_words)
wc.generate(text)
```

```python
type(wc.words_)
```

```python
keys = list(wc.words_.keys())
values = list(wc.words_.values())
for i in range(10):
    print(keys[i], ':', values[i])
```

> 기본 word cloud

```python
plt.figure(figsize=(10,6))
plt.imshow(wc, interpolation='bilinear')
plt.axis('off')
plt.show()
```

> 그림 위에 표시하기

```python
mask = np.array(Image.open(maskfile))
plt.imshow(mask, cmap=plt.cm.gray, interpolation='bilinear')
plt.axis('off')
plt.show()
```

```python
wc = WordCloud(background_color='white', mask=mask,
               max_words=1000, stopwords=stop_words)
wc.generate(text)
```

```python
plt.figure(figsize=(10,12))
plt.imshow(wc, interpolation='bilinear')
plt.axis('off')
plt.savefig(f'image_name.png')			# 그림 저장
plt.show()
```

