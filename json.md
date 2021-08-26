# json

## 예시 코드

```python
import json


person = {'id':1,
          'name':'danna',
          'school':[{'year':2013, 'school':'sunrin'},
                    {'year':2016, 'school':'konkuk'},]
          }
json_string = json.dumps(person, indent='\t')   # indent: 들여쓰기
person['name'] = 'kim'
with open('person.json', 'w', encoding='utf-8') as file:
    json.dump(person, file, indent='\t')

with open('person.json', 'r') as file:
    json_data = json.load(file)
print(json.dumps(json_data, indent='\t'))
```
