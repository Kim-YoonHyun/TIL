# 1. 개요

Linux 운영체제 하에서 CLI 기반 파일 전송 모듈

# 2. 명령어

## 2.1. 파일 전송

띄어쓰기에 주의

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

`-i`: identity file 지정

`-v`: 상세내용을 보면서 디버깅 시 

`-p`: 파일 수정 시간과 권한 유지(소문자 p 임)

### 

