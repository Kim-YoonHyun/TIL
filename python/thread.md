# 1. threading 모듈

## 1.1. 기본 import 방식

```python
import threading
```

## 1.2 명령어

### 1.2.1 thread 지정

```python
t = threading.Thread(target=<함수 or method>, options)
```

### 1.2.2 thread 실행

```python
t.start
```

### 1.2.3 예시

```python
def number_sum(a, b):
    print(a + b)

t = threading.Thread(target=number_sum, args=(10, 36))
# thread가 실행하는 함수(메서드)에 입력 파라미터를 전달 할 경우
# arg(키워드는 kwargs)로 전달.
t.start()
```

### 1.2.5 daemon thread

데몬 스레드로 지정된 서브 스레드는 메인스레드가 종료되면 그 즉시 같이 종료된다.

```python
t = threading.Thread(target=number_sum, args=(10, 36))
t.daemon = True
t.start()
```

### 1.2.6. 객체지향 예시

```python
class Worker(threading.Thread):
    def __init__(self, name):
        super().__init__()
        self.name = name            # thread 이름 지정

    def run(self):
        print("sub thread start ", threading.currentThread().getName(), end='\n')
        time.sleep(3)
        print("sub thread end ", threading.currentThread().getName(), end='\n')

print("main thread start")

for i in range(5):
    name = f'thread {i}'
    t = Worker(name)                # sub thread 생성
    t.start()                       # sub thread의 run 메서드를 호출
    time.sleep(0.1)
print("main thread end\n")
```

