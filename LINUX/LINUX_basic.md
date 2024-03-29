# 1. 기본개념

## 1.1 컴퓨터의 기본 구조

### 1.1.1. 하드웨어

- 컴퓨터, 키보드 등의 물리적인 것

### 1.1.2. 소프트웨어

1. 응용 프로그램: MSoffice, 웹 브라우저 등

2. 운영체제: Windows, **Linux**, ubuntu 등

   **2-1. 커널** : OS의 중추 역할이며 컴퓨터 자원을 관리. 사용자와의 직접적인 상호작용은 없음

   2-2. 시스템 프로그램 : 사용자와 상호작용을 위해 필요한 것. Shell 이 대표적

## 1.2. Linux 커널

### 1.2.1. 커널의 개념

- 운영체제(OS)의 가장 깊은 곳에서 작동되는 소프트웨어이며 OS의 핵심이다.
- **컴퓨터의 물리적 자원(하드웨어)과 추상화 자원을 관리**
  (추상화: 하나뿐인 하드웨어를 여러 사용자들이 번갈아 사용하게 중재함으로써 마치 여러개인 것 처럼 보여지도록 하는 것)
- **물리적 자원을 추상화하여 사용자가 보다 쉽게 접근 할 수 있도록 도와주는 것**

### 1.2.2. 커널의 주요 기능

1. 디바이스 관리
   : 디바이스 드라이버(하드웨어 입출력을 제어하는 소프트웨어)를 이용하여 장치를 관리
2. 프로세스(task) 관리
   : 프로그램을 실행할 때 파일 시스템 내 특정 디렉터리에 있는 프로그램의 파일을 읽어와 메모리에 적재. 메모리에서 프로그램이 종료되면 프로세스 삭제.
   **각 프로세스에 PID(Process ID)를 부여하여 관리**
3. 메모리 관리
   : 사용자 프로그램의 요구에 따라 **메모리 영역을 분배**하거나 **이용이 끝난 메모리 영역 회수** 및 **가상 메모리 지원**
4. 시스템 콜 제공
   : 약 300개의 시스템 콜 제공
   (시스템 콜: 표준 출력이나 파일을 쓰는 write, 읽어들이는 read, 프로세스 fork기능을 통해 사용자 프로그램에서 엑세스 할 수 있도록 함)
   (fork: 프로세스를 복제하는 방법)

### 1.2.3. 쉘

응용프로그램과 커널 사이에 위치하고 있으며 상호 대화를 하게 만들어준다.

명령어를 쉘에서 받아 해석하여 커널에 전달, 커널이 명령을 수행하고 그 결과가 다시 쉘을 거쳐서 사용자에게 도달.

#### Bash, Bourne Again Shell (/bin/bash)

현재 리눅스에서 표준으로 채택된 쉘. 



# 2. 설치

## 2.1. WSL 2

윈도우 OS 에서 리눅스를 사용할 수 있게 해주는 기능.

개발자 모드 등 설정 불필요.

1. 시작 버튼 옆에서 'window 기능 켜기/끄기'  검색

2. Linux용 Windows 하위 시스템 체크 



# 3. 명령어

### 1.3.1. 설명

```bash
man <명령어>	# <명령어> 에 대해 알 수 있음
```

### 1.3.2. 디렉토리

```bash
ls					# 현재 디렉토리
ls ~			 	# 루트 디렉토리의 파일 확인
pwd					# 현재 작업 중인 디렉토리
cd <디렉토리 이름>	# 작업 디렉토리 변경
mkdir <디렉토리 이름>	# 새로운 디렉토리 만들기
```

### 1.3.3. 파일

```bash
find						# 특정 파일을 찾을 때 (locate)와 같음
locate						# 특정 파일을 찾을 때 (find)와 같음
cp <대상 파일명> <사본 파일명> # <대상 파일명>의 사본을 <사본 파일명> 이름으로 생성
mv <대상 파일명> <변경 파일명> # <대상 파일명>의 이름을 <변경 파일명> 으로 바꿈
mv <대상 파일명> <~/path>    # <대상 파일명>을 path 디렉토리로 옮김
more					   # 파일을 내용을 한 번에 한 화면씩 보여줌
kill					   # 어플리케이션 실행 중지
ps <프로세스 ID> kill		 # 프로세스 ID에 해당하는 앱 실행 중지 
```



### 1.3.5. - , --

- 옵션이 문자 하나인 경우 `-`. `-a, -b, -h`
- 여러개의 옵션 한번에 지정 가능 `-abh`
- 옵션이 문자열인 경우 `--`. `--help`

예시

```bash
ls -l -S -G -r >>> ls -lSGr
```

### 1.3.6. ^

```bash
^c, ^d, ^\, ^z
```





