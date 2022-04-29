# 1. 개요

terminal 에서 파이썬을 실행할 때 인자값을 줄 수 있는 모듈.

# 2. 명령어

## 2.1. 기본 import

```python
import argparse
```

## 2.2. 파서 객체 생성

### 2.2.1. 기본 생성

```python
parser = argparse.ArgumentParser()
```

#### option - usage

- -h 옵션을 통해 볼 수 있는 usage 메세지를 재정의 할 수 있음
- 기본값: `filename.py [-h] ...`

```python
parser = argparse.ArgumentParser(usage='<message>')
```

#### option - description

- -h 옵션을 통해 프로그램에 대한 간략 설명 표시

```python
parser = argparse.ArgumentParser(description='<message>')
```

#### option - epilog

- -h 도움말 메시지의 마지막 부분에 나오는 설명

```python
parser = argparse.ArgumentParser(epilog='<message>')
```

#### option - action

- store_true 의 경우 default 는 False 이며 인자를 적어주면 True 가 저장됨
- store_false의 경우는 반대.

```python
parser.add_argument('--foo1', action='store_true')
parser.add_argument('--foo2', action='store_true')
parser.add_argument('--foo3', action='store_false')
parser.add_argument('--foo4', action='store_false')
args = parser.parse_args()

> python argparseTest.py --foo1 --foo4
args.foo: True
args.foo: False
args.foo: True
args.foo: False
```

#### option - metavar

- `help=` 에서 도움말 메세지를 생성할 때 표시되는 이름을 변경.

## 2.3. 인자 추가하기

- 인자는 기본적으로 str 임.

### 2.3.1. 선택인자

- optional 인자는 이름앞에 - 또는 -- 가 붙으며 기본적으로 필수로 지정할 필요 없음. (옵션으로 필수 지정가능)
- 보통 한 글자 약자(-)와 fullname(--) 을 하나씩 지정하는 편임.

```python
parser.add_argument('-o', '--optional_val')
```

- 터미널 실행 예시

```bash
$ python <filename>.py -o=abc
$ python <filename>.py --optional_val=asd
```

### 2.3.2. 위치인자

- 위치인자는 이름앞에 아무것도 없으며 실행시 필수로 지정해야함.
- 실행시 위치인자는 값을 

```python
parser.add_argument('positional_val')
```

- 실행 예시

```bash
$ python <filename>.py abc	# positional 만 지정됨
```



####  option - type

- type을 지정하지 않으면 기본적으로 문자열 반환

```python
parser.add_argument('-o', '--optional_val', type=int)
```

- 터미널 실행 예시

```bash
python <filename>.py -e=123
```

#### option - help

- 도움말 메세지에 인수의 설명을 추가

```python
parser.add_argument('-o', '--optional_val', help='위치변수입니다.')
```

#### option - default

- 인자에 기본값을 지정함

```python
parser.add_argument('-o', '--optional_val', type=int, default=1243)
```

## 2.4. 인자 활용

```python
args = parser.parse_args()
print(args.optional_val)
print(args.position_arg)
```



