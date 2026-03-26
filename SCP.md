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

### 옵션

`-r` : 폴더 전송

`-v` : 전송 상황 가시화

```bash
scp -P portB -v -r userB@ipB:/path/to/serverB/file userA@ipA:/path/to/serverA/destination
```

## 2.2. key 방식

python subprocess 를 통해 자동화를 하려는 경우 '비밀번호입력' 을 직접제어하는게 불가능해서

SSH Key 기반 방식을 활용하는 것이 권장된다.

- 서버A에서 실행, 서버A --> 서버B

1. 서버A 에서 서버B 접속 전용 SSH key 생성
   ```bash
   ssh-keygen -t rsa -b 4096 -N "" -f ~/.ssh/id_rsa_for_server_b
   ```

   `ssh-keygen` : SSH 인증용 key 생성 도구

   `-t rsa` : 암호화 알고리즘의 종류 지정. RSA 는 가장 표준 방식

   `-b 4096` : key 의 길이 (bits) 설정. 4096 은 매우 높은 수준의 보안 강도

   `-N ""` : 추가 암호를 설정하지 않겠다는 의미. 빈 값을 주어야한 사람이 암호를 직접 입력하는 과정이 없어짐

   `-f ~/.ssh/id_rsa_<serverb name>` : 생성될 key 의 파일 경로와 이름 지정. 이름을 따로 지정하지 않으면 `id_rsa` 라는 기본값으로 생성되고 이는 기존의 key 를 덮어쓸 위험 존재

   - 생성된 key 확인
     ```bash
     ls -l ~/.ssh
     ```

2. 서버B 에 공개키 등록
   ```bash
   ssh-copy-id -i ~/.ssh/id_rsa_<name>.pub <userB>@<ipB>
   ssh-copy-id -p 1234 -i ~/.ssh/id_rsa_<name>.pub <userB>@<ipB>
   ```

   `ssh-copy-id` : 로컬(서버A)의 공개 키를 원격 서버(서버B) 의 `/.ssh/authorized_keys` 파일에 자동으로 추가해주는 유틸리티

   `-p 1234` : 접속할 서버B 의 포트가 자동(22) 이 아니라 직접 지정하는 경우

   `-i <경로>/<key명칭>.pub` : 보낼 공개키 파일의 경로 지정. 이때 반드시 `.pub` 으로 끝나는 파일을 지정

   `<userB>@<ipB>` : 접속할 서버B 의 사용자 ID 및 IP 값.

   - 옮겨진 key 확인(서버B 에서 진행)
     ```bash
     cat ~/.ssh/authorized_keys
     ```

3. 전송 진행
   ```bash
   scp -P <portB> \
   	-i /home/user/.ssh/id_rsa_for_server_b, # 전용 비밀키 지정
   	-o "StrictHostKeyChecking=no", # 첫 접속 시 신뢰 확인 생략
       <기존 scp 전송 명령어>
   ```

   









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

