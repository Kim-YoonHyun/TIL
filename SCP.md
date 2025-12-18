# 1. 개요

Linux 운영체제 하에서 CLI 기반 파일 전송 모듈

# 2. 명령어

## 2.1. 파일 전송

서버A : 111.111.111.111 port 2010

서버B : 222.222.222.222 port 2020

서버C : 123.123.123.123 port 2030

- 서버A 에서 실행, 서버B --> 서버A
  ```bash
  scp -P portB userB@ipB:/path/to/serverB/file /path/to/serverA/destination
  # scp -P 2020 userB@222.222.222.222:/home/data/backup.tar.gz /home/data/backup/
  ```

- 서버A 에서 실행, 서버B --> 로컬
  ```bash
  불가능
  ```

- 서버B 에서 실행, 서버B --> 서버A

  ```bash
  scp -P portA /path/to/serverB/file userA@ipA:/path/to/serverA/destination
  ```

- 서버B 에서 실행, 서버B --> 로컬
  ```bash
  scp /path/to/serverB/file userLocal@LOCAL_IP:/path/to/local/destination
  ```

- 서버C 에서 실행, 서버B --> 서버A
  ```bash
  scp -P portB userB@ipB:/path/to/serverB/file userA@ipA:/path/to/serverA/destination
  ```

  단, 접근 권한 필요

### 2.1.1. 자동화

python subprocess 를 통해 자동화를 하려는 경우 '비밀번호입력' 을 직접제어하는게 불가능해서

SSH Key 기반 방식을 활용하는 것이 권장된다.

- 서버A에서 실행, 서버B --> 서버A

1. 서버A SSH key 생성

### 2.1.1. 로컬 -> 원격

```bash
$ scp <전송할 파일 경로> <유저명>@<IP주소>:<받을 경로>
```

예시: example.txt (로컬)를 test 디렉토리(원격)로.

```bash
$ scp /home/example.txt kyh@123.456.xxx.xxx:/home/test
```

### 2.1.2. 원격 -> 로컬

```bash
$ scp <유저명>@<IP주소>:<전송할 파일 경로> <받을 경로>
```

예시: test.txt (원격) 를 example 디렉토리(로컬) 로.

```bash
$ scp kyh@123.456.xxx.xxx:/home/test.txt /home/example
```

### 2.1.3. 원격 -> 원격

서버A : 111.111.111.111 port 2010

서버B : 222.222.222.222 port 2020

- 서버 A 에서 명령어를 실행하여 서버B 의 파일을 서버A 로 가져올때
  ```bash
  scp -P 2020 userB@222.222.222.222:<서버B/파일/경로> <서버A/저장/경로>
  ```

  

```bash
scp -P 202 -r <유저id>@<ip>:<폴더경로> <유저id>@<보낼ip>:<폴더상위경로>
```

```bash
scp -P 2020 -r kimyh@111.111.111.111:/home/kimyh/python/project jyp@222.222.222.222:/home/kimyh/python/
```



```bash
$ scp <유저명>@<IP주소>:<전송할 파일 경로> <유저명>@<IP주소>:<받을 경로>
```

예시: test .txt (원격)를 example디렉토리(원격)로.

```bash
$ scp kyh@123.456.xxx.xxx:/home/test.txt kyh@456.789.xxx.xxx/home/example.txt 
```

## 2.2. 파일 전송 옵션

### 2.2.1. 옵션 활용법

```bash
$ scp <옵션> <파일 전송>
```

### 2.2.1. 옵션 종류

`-r` :폴더를 복사. 

`-P`: ssh 포트 번호 지정 복사(대문자 P임)

```bash
$ scp -P 1122 /home/example.txt kyh@123.456.xxx.xxx:/home/test
```

`-i`: identity file 지정

`-v`: 상세내용을 보면서 디버깅 시 

`-p`: 파일 수정 시간과 권한 유지(소문자 p 임)

### 

