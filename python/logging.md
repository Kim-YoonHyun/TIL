# 1. 개요

# 2. 명령어

## 2.1. 요약

```python
import logging
logger = logging.getLogger()
logger.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(message)s')

stream_handler = logging.StreamHandler()
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)

file_handler = logging.FileHandler(f'my_log.log')
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
```

```python
logger.info('aaa')
```



## 2.2. 기본 import

```python
import logging
```

## 2.3. logger 객체 생성

```python
logger = logging.getLogger(name=None)
```

#### option - name

`name=None(기본값)` 인 경우 계층의 루트 로거를 돌려준다.

`name`을 설정할 경우 지정된 이름의 로거를 돌려줌.

## 2.4. logging 수준 설정

- 설정한 level 보다 심각한 로깅 메세지만 출력됨.

```python
logger.setLevel(logging.<level>)
```

- level

  | 수준             | 숫자 값 |
  | ---------------- | ------- |
  | CRITICAL         | 50      |
  | ERROR            | 40      |
  | WARNING(default) | 30      |
  | INFO             | 20      |
  | DEBUG            | 10      |
  | NOTSET           | 0       |

  

## 2.5. formatter

```python
formatter = logging.Formatter('%(asctime)s - %(name)s - %(message)s')
```

- logRecord 
  | 포맷                | 설명                                                         |
  | ------------------- | ------------------------------------------------------------ |
  | `%(asctime)s`       | logRecord 가 생성된 시간. ‘2003-07-08 16:49:45,896’  형식.   |
  | `%(created)f`       | logRecord 가 생성된 시간. '1654562021.234252' 형식.          |
  | %(funcName)s        | 로깅 호출을 포함하는 함수의 이름                             |
  | %(lenelname)s       | 로깅 수준                                                    |
  | %(pathname)s        | 로깅 호출이 일어난 소스 파일의 전체 경로 명                  |
  | `%(filename)s`      | pathname 에서의 파일 명                                      |
  | %(module)s          | filename 의 이름 부분                                        |
  | %(lineno)d          | 로깅 호출이 일어난 소스 행 번호                              |
  | %(message)s         | 로그 된 메시지                                               |
  | %(process)d         | 프로세스 id                                                  |
  | %(processName)s     | 프로세스 이름                                                |
  | %(relativeCreated)d | logging 모듈이 로드된 시간을 기준으로 logRecord 가 생성된 시간. |
  | %(thread)d          | 스레드 ID                                                    |
  | %(threadName)s      | 스레드 이름                                                  |

  

## 2.6. handler

log 메세지의 level 예 따라 적절한 log 메세지를 지정된 위치에 전달 하는 역할.

### 2.6.1. StreamHandler

Stream (console) 에 메세지 전달.

```python
stream_handler = logging.StreamHandler()
stream_handler.setFormatter(formatter)
logger.addHandler(stream_handler)
```

### 2.6.2. FileHandler

파일 (.log 등) 에 메세지 전달

```python
file_handler = logging.FileHandler(f'my_log.log')
file_handler.setFormatter(formatter)
logger.addHandler(file_handler)
```

### 2.6.3. TimedRotatingFileHandler

시간을 정해서 log 생성, 제거.

```python
TimedRotatingFileHandler(
	filename,  
    when='h',
    interval=1,
	backupCount=0,
    encoding='utf-8,
	delay=False,  
    utc=False, 
    atTime=None
    )
```

#### option - filename

저장할 로그 파일 이름

#### option - when

`interval` 의 유형 지정

| 값          | 유형                       | atTime 적용 여부             |
| ----------- | -------------------------- | ---------------------------- |
| 'S'         | 초                         | 적용되지 않음                |
| 'M'         | 분                         | 적용되지 않음                |
| 'H'         | 시간                       | 적용되지 않음                |
| 'D'         | 일                         | 적용되지 않음                |
| 'W0' - 'W6' | 요일(월:0 ~ 일:6)          | 최초 롤오버 시간 계산에 적용 |
| 'midnight'  | 자정 (atTime 에 따라 변경) | 최초 롤오버 시간 계산에 적용 |
