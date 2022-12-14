# 1. 개요

매달, 매달 특정일, 매일, 매일 특정 시간,분,초 등등

원하는 시간에 python script 를 실행하고 싶을 때 사용하는 스케줄러

# 2. 명령어

## 2.1. install

```python
pip install APScheduler
```

## 2.2. 기본 import

### 2.2.1. BlockingScheduler

단일 실행

```python
from apscheduler.schedulers.blocking import BlockingScheduler
sched = BlockingScheduler(timezone='Asia/Seoul')
```

### 2.2.2. BackgroundScheduler

백그라운드 실행

```python
from apscheduler.schedulers.background import BackgroundScheduler
sched = BackgroundScheduler()
```

## 2.3. interval 

### 2.3.1. () 초마다 실행

```python
@sched.scheduled_job('interval', seconds=5, id='test_1')
def job1():
    print(f'job1 : {time.strftime("%H:%M:%S")}')
sched.start()
```

### 2.3.2. () 분마다 실행

```python
@sched.scheduled_job('interval', minute=25, id='test_1')
def job1():
    print(f'job1 : {time.strftime("%H:%M:%S")}')
sched.start()
```

### 2.3.3. () 시간마다 실행

```python
@sched.scheduled_job('interval', hour=1, id='test_1')
def job1():
    print(f'job1 : {time.strftime("%H:%M:%S")}')
sched.start()
```



## 2.4. cron

### 2.4.1. 특정 시간대에 실행

```python
# 매일 12시 30분에 실행
@sched.scheduled_job('cron', hour='12', minute='30', id='test_2')
def job2():
    print(f'job2 : {time.strftime("%H:%M:%S")}')
    
sched.start()
```

```python
# 매일 [9시15분, 9시45분, 15시15분, 15시45분]에 실행
@sched.scheduled_job('cron', hour='9,15', minute='15,45', id='test_2')
def job2():
    print(f'job2 : {time.strftime("%H:%M:%S")}')
```



## 2.5. 추가

```python
def job2():
    print(f'job2 : {time.strftime("%H:%M:%S")}')

sched.add_job(job2, 'cron', second='0', id="test_3")
sched.start()
```

