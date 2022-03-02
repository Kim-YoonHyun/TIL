# 1. 개요

terminal 에서 파이썬을 실행할 때 인자값을 줄 수 있는 모듈.

# 2. 명령어

## 2.1. 기본 import

```python
import argparse
```

## 2.2. 파서 객체 생성

```python
parser = argparse.ArgumenParser()
```

## 2.3. 인자 추가하기

```python
parser.add_argument('-o', '--optional_val')
# 터미널 실행시 -o 또는 --optional_val 로 인자 지정 가능
```

- 터미널 실행 예시

```bash
python <filename>.py -o=예시
python <filename>.py --optional_val=예시
```

### 2.3.1. type 지정하기

type을 지정하지 않으면 기본적으로 문자열 반환

```python
parser.add_argument('-o', '--optional_val', type=int)
```

- 터미널 실행 예시

```bash
python <filename>.py -e=123
```

### 2.3.2. optional & positional

-  optional 인자는 이름앞에 - 또는 -- 가 붙으며 기본적으로 필수로 지정할 필요 없음. (옵션으로 필수 지정가능)
- positional 인자는 이름앞에 아무것도 없으며 실행시 필수로 지정해야함.

```python
parser.add_argument('-o', '--optional_val')
parser.add_argument('positional_val')
```

- 터미널 실행 예시

```bash
python <filename>.py abc	# positional 만 지정됨
python <filename>.py -o=abc efg	  # optional, positional 둘다
python <filename>.py -o=abc  # optional만 지정되어 에러 발생.

```







