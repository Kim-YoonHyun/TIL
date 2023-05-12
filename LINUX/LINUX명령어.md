# 1. 개요

# 2. 명령어

## 2.1. 디렉토리 관련

### 2.1.1. 작업 디렉토리 확인

```bash
$ pwd
```

### 2.1.2. 경로 이동

```bash
$ cd 
```

### 2.1.3. 파일 리스트

```bash
$ ls
```

### tmux 설치



### 2.1.1. 디렉토리 생성

```bash
mkdir aa
```

aa라는 이름의 디렉토리 폴더 생성.

### 2.1.2. -p 

중간 디렉토리 생성.

```python
mkdir -p a/aa
```

a 라는 중간 경로용 폴더를 자동으로 생성 후 aa 라는 이름의 디렉토리 폴더 생성.
-p 옵션이 없는 경우 중간 경로 폴더인 a 가 없어서 에러 발생.

### 2.1.3. ~

```bash
mkdir ~ aa
```

## 2.1.4. 용량확인

#### 파일의 용량 확인

```bash
$du
```

#### option

```bash
-s # 선택한 디렉토리만의 용량
-h # 읽기편한 단위
```

```bash
$du -sh /<디렉토리>/*
```

```bash
$du -h --max-depth=1
```

#### 남은 용량 확인

```bash
$df -k
```







## Anaconda 설치

[참고 블로그](https://dambi-ml.tistory.com/6)

설치 후 재접속해야 적용됨.

```bash
netstat -lntp
ps -ef | grep python
kill -9 <pid>
killall python3
```

## 2.2. 포트

### 2.2.1. 열린 포트 확인

```bash
$netstat -ltup
$netstat -nap
$ss -lntu
```

### 2.2.2. 특정 포트 상태 확인

```bash
netstat -nap | grep <포트번호>
```

### 2.2.3. 특정 포트 열기

```bash
$firewall-cmd --permanent --zone=public --add-port=8080/tcp
$firewall-cmd --reload

prodigy ner.manual 0001 blank:en data_0001_8050.txt --label label1,label2,label3 --highlight-chars
```

### 2.2.4. 백그라운드 실행

```bash
command > /dev/null 2>&1 &
```

## 2.3. 사양 확인

### 2.3.1. kernel 확인

```bash
cat /proc/sys/kernel/osrelease
```

### 2.3.1. 운영 체제 확인

```bash
cat /etc/issue.net
```

### 2.3.2.

```bash
```

