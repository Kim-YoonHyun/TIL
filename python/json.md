# json

## 예시 코드

### 기본 형태

```python
import json
person = {'id':1,
          'name':'danna',
          'school':[{'year':2013, 'school':'sunrin'},
                    {'year':2016, 'school':'konkuk'},]
          }
```

#### json -> string 변환

```python
json_string = json.dumps(person, indent='\t')   # indent: 들여쓰기
```

#### 값 추가 & 변환

저장되어있는 객체의 스타일(dict, list 등)에 맞춰 해당 문법을 통해 추가dict()

```python
person['name'] = 'kim'
```

```python
person['school'].append({})
```

#### json 파일 저장

```python
with open('person.json', 'w', encoding='utf-8') as file:
    json.dump(person, file, 
              indent='\t',	# 들여쓰기 옵션
              ensure_ascii=False	# 한글로 저장
             )
```

#### json 파일 dict 객체로 불러오기

```python
with open('person.json', 'r', encoding='utf-8') as file:
    json_data = json.load(file)
json_string = json.dumps(json_data, indent='\t')
```

### 데이터 추가

```python
person
```

