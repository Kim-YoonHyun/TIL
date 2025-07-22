# NLP(Natural Language Processing)

[관련 사이트](https://wikidocs.net/book/2155)

## toolkit

- 대표적인 툴킷
  - NLTK (Natural Language Processing Toolkit)

- 한국어 처리 패키지
  1. KoNLPy (코엔엘파이)
  2. Hannanum (한나눔)
  3. Khma (꼬꼬마)
  4. Komoran (코모란)
  5. Mecab (은전한잎)
  6. OKT (이전의 Twitter)

### nltk

#### 단어 토큰화

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
```

```python
text1 = "Don't be fooled by the dark sounding name, Mr. Jone's Orphanage is as cheery as cheery goes for a pastry shop."
text2 = "Starting a home-based restaurant may be an ideal. it doesn't have a food chain or restaurant of their own."

from nltk.tokenize import word_tokenize 
word_tokenize(text1)

from nltk.tokenize import WordPunctTokenizer  
WordPunctTokenizer().tokenize(text1)

from nltk.tokenize import TreebankWordTokenizer
TreebankWordTokenizer().tokenize(text2)
```

```python
from tensorflow.keras.preprocessing.text import text_to_word_sequence
print(text_to_word_sequence(text1))
```

#### 문장 토큰화

```python
text="His barber kept his word. But keeping such a huge secret to himself was driving him crazy. Finally, the barber went up a mountain and almost to the edge of a cliff. He dug a hole in the midst of some reeds. He looked about, to make sure no one was near."

from nltk.tokenize import sent_tokenize
sent_tokenize(text)
```

#### 표제어 추출

```python
nltk.download('wordnet')
```

```python
from nltk.stem import WordNetLemmatizer
n=WordNetLemmatizer()
n.lemmatize('dies','v'), n.lemmatize('has','v'), n.lemmatize('watched','v')
```

#### 어간 추출

```python
text="This was not the map we found in Billy Bones's chest, but an accurate copy, complete in all things--names and heights and soundings--with the single exception of the red crosses and the written notes."

from nltk.stem import PorterStemmer
from nltk.tokenize import word_tokenize
s = PorterStemmer()
words=word_tokenize(text)
[s.stem(w) for w in words]
```

#### 불용어(Stopwords)

```python
from nltk.corpus import stopwords
len(stopwords.words('english'))
```

```python
example = "Family is not an important thing. It's everything."
stop_words = set(stopwords.words('english')) 
stop_words.add("'s")        # 추가하고 싶으면 추가하면 됨

word_tokens = word_tokenize(example.lower())
result = [w for w in word_tokens if w not in stop_words]

print(word_tokens) 
print(result) 
```

```python
example = "고기를 아무렇게나 구우려고 하면 안 돼. 고기라고 다 같은 게 아니거든. 예컨대 삼겹살을 구울 때는 중요한 게 있지."
stop_words = "아무거나 아무렇게나 어찌하든지 같다 비슷하다 예컨대 이럴정도로 하면 아니거든"
# 위의 불용어는 명사가 아닌 단어 중에서 저자가 임의로 선정한 것으로 실제 의미있는 선정 기준이 아님
stop_words=stop_words.split()
word_tokens = word_tokenize(example)

result = [w for w in word_tokens if w not in stop_words]

print(word_tokens) 
print(result)
```



#### 품사(POS) 태깅

```python
nltk.download('averaged_perceptron_tagger')
from nltk.tag import pos_tag
x = word_tokenize(text)
pos_tag(x)
```

### KoNLPy

```python
!pip install Konlpy 
from konlpy.tag import Okt
```

#### 형태소 분석

```python
Okt().morphs('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
```

#### 품사 부착

```python
Okt().pos('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
```

#### 명사 추출

```python
Okt().nouns('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
```

### Kkma

```python
from konlpy.tag import Kkma
Kkma().morphs('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
Kkma().pos('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
Kkma().nouns('열심히 코딩한 당신, 연휴에는 여행을 가봐요')
```



