# 예시 구조

system baseline 과 동일

```python
config.ini
utils/temp.py
system/
├── requirements.txt
├── Dockerfile
├── .dockerignore
├── build/stage/src/...
└── src/
    ├── module1/run.py
    ├── module2/run.py
    └── start.sh

```

docker 컨테이너는 자체적인 파일 시스템을 가지기 때문에 호스트 서버에서 개발시 지정한 절대경로
(예: /home/user/module/code.py) 는 docker 컨테이너안에 존재하지 않음. 

해결방법은

1. 상대경로로 코드 수정
2. docker 의 경로를 호스트의 절대경로와 완전 동일하게 설정

두 번째 방법은 docker 의 경로 유연성을 해치기에 권장하지 않음

상대 경로 설정 방법

```python
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
json_path = os.path.join(BASE_DIR, "data_module", "supports", "supports.json")
```

# 설치

| 명령어                  | 관리자          | 장점                     | 단점                       |
| ----------------------- | --------------- | ------------------------ | -------------------------- |
| `snap install docker`   | Ubuntu 제조사   | 가장 간편, 자동 업데이트 | 파일 경로 제약             |
| `apt install docker.io` | Ubuntu 커뮤니티 | 별도 설정 없이 바로 설치 | 공식버전보다 업데이트 느림 |
| `apt install docker-ce` | Docker 공식     | 가장 표준, 최신          | 설치가 복잡                |

## docker-ce

1. 구버전 제거(없으면 무시)
   `sudo apt-get remove docker-engine docker.io containerd runc`

2. 필수 패키지 설치
   ```cmd
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg lsb-release
   ```

3. GPG 키 등록
   다운로드하려는 파일이 Docker 공식 파일이 맞는지 검증하는 키 등록

   ```bash
   sudo mkdir -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   ```

4. 공식 저장소 추가
   ```bash
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

5. Docker 엔진 설치
   ```bash
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

6. 사용자 권한 부여(sudo 없이 사용 위함)

   ```bash
   sudo usermod -aG docker $USER
   # 적용을 위해 터미널을 껐다 켜거나 아래 명령 실행
   newgrp docker
   ```

7. 부팅 시 자동 실행 설정
   ```bash
   sudo systemctl enable docker
   sudo systemctl start docker
   ```

   

# 순서

```bash
# user 계정에서 권한 확인
docker info
```

## 환경 설정 추출

- conda 환경을 사용 중인 경우
  ```bash
  conda env export > environment.yml
  ```

- pip 패키지를 설정할 경우
  ```bash
  pip freeze > requirements.txt
  ```

## 1. Dockerfile 생성

Dockerfile 이 존재하는 디렉토리에서 docker 이미지를 생성할 경우 상위 디렉토리=app 이다.
즉, 상위 폴더를 하나의 이미지로 뭉치는게 아니라 dockerfile 이 존재하는 공간 자체가 app 아래에 들어간다고 보면 된다. 경로로 표현하면 `app/src/` 과 동일함.
즉 `CMD ["python", "src/module1/run.py"]` 으로 넣으면 `/app/src/module1/run.py` 를 찾는것이 되므로 에러가 난다.

### pip (추천)

docker 를 할때는 conda 를 사용하면 너무 이미지가 무거워 지므로 pip 로 변경하는 것이 추천됨

```bash
# 베이스 이미지
FROM python:3.10-slim

# 작업 디렉토리 설정
WORKDIR /app

# 라이브러리 설치
COPY requirements.txt .
COPY stage/src/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 전체 코드 복사
# Host 폴더들을 컨테이너 내부 /app/ 하위로 복사
COPY ./src ./src
COPY ./configs ./configs
COPY ./resources ./resources
COPY stage/src ./src
COPY stage/configs ./configs
COPY stage/resources ./resources
COPY ipconfig.ini .
COPY ipconfig_cryp.ini .
COPY cryptogram ./cryptogram

# 루트 경로 설정
ENV PYTHONPATH=/app/src
ENV PYTHONPATH=/app/src:/app # : 은 구분자


# 일반 실행 명령
CMD ["python", "-m", "analy_module.run"]
# FastAPI 실행 명령
# CMD ["uvicorn", "whole_module.run_api:app", "--host", "0.0.0.0", "--port", "8000"]

```

### conda (비추천)

conda 가상환경은 이미 격리된 docker 에 또다시 가상환경을 넣는 형태라 비효율적이고 무거워지므로 비추천

```bash
# Step 1: 베이스 이미지
FROM continuumio/miniconda3

# Step 2: 작업 디렉토리 설정 (app 은 디렉토리 명이 아니라 진짜 app 으로 적음)
WORKDIR /app

# 환경 변수 설정
ENV PYTHONPATH=/app

# Step 3: 전체 모듈 복사
COPY . .

# Step 4: conda 환경 생성
RUN conda env create -f environment.yml

# Step 5: conda 환경 활성화 설정
SHELL ["conda", "run", "-n", "<가상환경이름>", "/bin/bash", "-c"]

# Step 6: 실행
CMD ["conda", "run", "-n", "<가상환경이름>", "python", "run_api.py"]
```

<가상환경이름> 은 environment.yml 의 name 에 명시되어있는 환경 이름이어야함.



## 2. .dockerignore 생성

```dockerfile
# 예시
__pycache__/
*.pyc
*.pyo
*.pyd
*.db
.env
.git

/src/module2
# 그외 폴더&파일 필요시 입력
```

## 3. image 빌드

```bash
cd system
docker build -t <image name> .  # . 있음 주의
docker build -t <image name>:<version> .  # 버전은 : 을 통해 하나만 붙일 수 있고 :ver:ver: ... 식으로 2개 이상 불가
docker build --no-cache -t <image_name> . # 레이어 캐시 무시 
```

### 3-1. image 확인

```bash
docker images
```

## 4. 실행

```bash
docker run --rm <image name>
docker run -p <port>:<port> <image name>
docker run --rm -p 7010:7010 <image name> # api 를 실행하는 경우
# --rm : 프로세스가 끝나면 컨테이너 정보를 삭제하는 것. 
```

### 마운트

데이터의 값 자체가 아예 API 나 그외 입출력방법에 의해 모듈에 들어오는 경우는 문제가 없지만
모듈 자체에서 어딘가에 저장되어있는 데이터를 불러오려는 경우, 해당 데이터는 docker 이미지에 포함되어있어야만 불러올 수 있음

예:) 데이터값 자체를 API 모듈에 입력 = 문제없음

데이터가 존재하는 path 정보를 api 모듈에 입력 후, API 모듈 내부에서 해당 데이터를 읽기 --> docker 이미지에 해당 데이터가 포함되어있지 않으면 읽기 불가능

마운트를 하면 docker 는 기본적으로 격리가 원칙이지만 상황에 따라 호스트의 **실제파일**과 연결하는 것이 가능

```bash
docker run --rm \
	-v /host/path/to/config.ini:/config.ini:ro \ 	# app 보다 상위 이므로 그냥 /로 설정
	-v /host/path/to/utils:/utils:ro \
	-v /etc/localtime:/etc/localtime:ro \  # 시간 정보 파일 연결
	-v /etc/timezone:/etc/timezone:ro \ # 시간대 이름 설정을 연결
	<image name>
# 볼륨 마운트를 통해 외부 파일 연결하기
```

docker 는 내부적으로 독립된 시간 체계가 흘러가므로 run 시 호스트와 시간을 동일하게 하거나 dockerfile 자체에서 시간대를 설정해줄 필요가 있음

### 저장

docker 내부의 가상 파일 시스템에 저장되며 <작업디렉토리>/<설정한경로> 에 저장된다고 보면 됨.
이 경우 host 에게는 파일을 가시적으로 볼 수 있는 방법은 없으며 docker 컨테이너가 삭제될 경우 같이 삭제됨.

만약 외부에 저장하고 싶은 경우는 볼륨 마운트 필요

```bash
docker run -v /host/input_data:/app/input_data:ro \
           -v /host/output_data:/app/output_data:rw \
           mobility_api
# ro: 읽기전요
# rw 읽기/쓰기 (기본값)
```

docker 컨테이너는 내부적으로 `app/input_data` 를 읽지만 실제로는 `/host/input_data` 와 대응되도록 하는 것

이때, 볼륨 마운트를 하는 경우 docker 컨테이너 내부적으로 설정하는 경로는 **임의의 아무경로**를 설정해도 상관없음.
마구잡이식의 이상한 경로 (예: /aaa/bbb/ccc/hafga/) 가 들어가도 실제로 읽는건 마운트된 호스트의 경로이기 때문.
하지만, 이상한 경로값을 마음대로 넣어버리면 혼란을 야기하고 디버깅 시간이 걸리므로 내부적인 규칙을 잘 지정하는 것이 중요함

# docker hub

만든 이미지를 서버에 올리고 어디서든 다시 내려받을 수 있도록 해주는 클라우드 기반 이미지 저장소. Github 와 같은 용도

## 순서

1. docker 이미지 생성

2. 서버 터미널에서 로그인
   ```bash
   docker login -u <username>
   ```

   ```bash
   docker login -u kimyh663927
   ```

   패스워드는 토큰 사용

3. 이미지에 tag 붙이기
   ```bash
   docker tag <image name> <docker username>/<repo name>:<tag>
   ```

   ```bash
   docker tag new_image:b3.0.1 kimyh663927/new_image:v1.0
   docker tag new_image:b3.0.1 kimyh663927/my_repo:v1.0
   ```

   이때 `<docker username>` 은 **실제 docker hub 계정의 username 과 일치**해야한다.
   `<repo name>` 은 혼란 방지를 위해 push 할 `docker image` 명칭을 쓰는 경우가 많지만 실제로는 docker hub 에 생성할 레파지토리 명칭에 해당된다. 뒤에 붙이는 `<tag>` 는 docker hub 에서의 버전 태그에 해당되며 이미지의 실제 버전 명시와는 아무런 관계가 없다.

4. push 진행
   ```bash
   docker push <docker username>/<repo name>:<tag>
   ```

   ```bash
   docker push kimyh663927/new_image:v1.0
   docker push kimyh663927/my_repo:v1.0
   ```

   이때 username 으로 로그인 한 상태라면 로그인했을때의 token 이 Read-only 인지 write, Delete 까지 허용된 token 인지 확인해야한다.

   push 할때 private 설정은 할 수 없으며 hub 사이트에 들어가서 미리 레파지토리를 private 로 만들어두던가 push 후 직접 hub 사이트에서 레파지토리 설정을 변경해야한다.

5. pull 진행 (서버에서 사용시)
   ```bash
   docker pull <docker username>/<image name>
   ```

   ```bash
   docker pull kimyh663927/new_image:v1.0
   ```

6. 이후 docker image 사용 방식과 동일

# 기타

## 이미지 삭제

```bash
docker rmi mobility_api # 이름으로 삭제
docker rmi a1b2c3d3e4f5 # ID 로 삭제
```

## 컨테이너

- 확인

```bash
docker ps -a
```

- 중지&삭제

  ```bash
  docker stop <ID>
  docker rm <ID>
  ```

## 디버깅(임시)

```bash
docker run -it mobility_api bash
```

## 캐시 제어

만약 aaa 라는 docker 이미지를 만든 다음 삭제하고, 다시 aaa 를 만들면 pip install 없이 빠르게 진행되는 것을 볼 수 있음.
이는 docker layer cache 를 재활용하기 때문임

```dockerfile
FROM python:3.10         ← 레이어 1
WORKDIR /app             ← 레이어 2
COPY requirements.txt .  ← 레이어 3
RUN pip install -r requirements.txt  ← 레이어 4
COPY . .                 ← 레이어 5
CMD ["python", "main.py"]

```

만약 코드만 수정되었다면 COPY . . 만 수행되고 나머지 레이어에 대해서는 캐시된 레이어를 그대로 재사용함.

이러한 현상을 해결하기 위해선 아래와 같은 방법이 있음

1. 캐시 무시 빌드
   ```bash
   docker build --no-cache -t <image_name> .
   ```

## 이미지 저장(export)

```bash
# 단순 추출
docker save -o <my_image>.tar <my_image>:<tag>
# 압축하는 경우
docker save <myapp>:<tag> | gzip > <myapp>.tar.gz
# 진행률 확인
docker save <myapp>:<tag> | pv | gzip > <myapp>.tar.gz

```

tag 는 의미 그대로 태그를 의미하며 명시하고자하는 버전 등을 적으면 됨 (예: latest)

## 이미지 불러오기

```bash
docker load -i <myapp>.tar
# 압축된 파일의 경우
gunzip -c <myapp>.tar.gz | docker load
# 진행률 확인
gunzip -c <myapp>.tar.gz | pv | docker load
```

## \<none> 삭제

```bash
docker image prune
```

