# Docker 이미지화 하는 방법

예제 구조

```python
project/
└── whole_module/
    ├── requirements.txt
    ├── data_module/
    ├── rag_module/
    ├── run_api.py
    ├── Dockerfile
    └── .dockerignore
```

docker 컨테이너는 자체적인 파일 시스템을 가지기 때문에 호스트 서버에서 개발시 지정한 절대경로
(예: /home/user/module/code.py) 는 docker 컨테이너안에 존재하지 않음. 

해결방법은

1. 상대경로로 코드 수정
2. docker 의 경로를 호스트의 절대경로와 완전 동일하게 설정

두 번째 방법은 docker 의 경로 유연성을 해치기에 권장하지 않음

상대 경로 설정 방법

```python
BASE_DIR = os.path.dirname(os.path.abspath(__file__))  # run_api.py 기준 디렉토리
json_path = os.path.join(BASE_DIR, "data_module", "supports", "supports.json")
```



## 순서

### (권한문제가 있는 경우) 권한 설정

```bash
# docker 그룹에 user 포함
sudo usermod -aG docker <user>

# user 재로그인
su -<user>

# user 계정에서 권한 확인
docker info
```



### 환경 설정 추출

- conda 환경을 사용 중인 경우
  ```bash
  conda env export > environment.yml
  ```

- pip 패키지를 설정할 경우
  ```bash
  pip freeze > requirements.txt
  ```

### Dockerfile 생성 - pip

```dockerfile
# 베이스 이미지
FROM python:3.10-slim

# 작업 디렉토리 설정
WORKDIR /app

# 루트 경로 설정
# ENV PYTHONPATH=/app 

# 종속성 복사 및 설치
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 전체 코드 복사
# whole_module/ 안의 모든 파일을 /app 에 복사
COPY . .

# 일반 실행 명령
CMD ["python", "run_api.py"]
# FastAPI 실행 명령
# CMD ["uvicorn", "whole_module.run_api:app", "--host", "0.0.0.0", "--port", "8000"]

```



### Dockerfile 생성 - conda

```dockerfile
# Step 1: 베이스 이미지
FROM continuumio/miniconda3

# Step 2: 작업 디렉토리 설정 (app 은 디렉토리 명이 아니라 진짜 app 으로 적음)
WORKDIR /app

# Step 3: 전체 모듈 복사
COPY . .

# Step 4: conda 환경 생성
RUN conda env create -f environment.yml

# 환경 변수 설정
ENV PYTHONPATH=/app

# Step 5: conda 환경 활성화 설정
SHELL ["conda", "run", "-n", "<가상환경이름>", "/bin/bash", "-c"]

# Step 6: FastAPI 실행
CMD ["conda", "run", "-n", "<가상환경이름>", "python", "run_api.py"]
```

<가상환경이름> 은 environment.yml 의 name 에 명시되어있는 환경 이름이어야함.

Dockerfile 이 존재하는 디렉토리에서 docker 이미지를 생성할 경우 상위 디렉토리=app 이다.
즉, 상위 폴더를 하나의 이미지로 뭉치는게 아니라 dockerfile 이 존재하는 공간 자체가 app 아래에 들어간다고 보면 된다. 경로로 표현하면 `app/run_api.py` 과 동일함.
즉 `CMD ["python", "whole_module/run_api.py"]` 으로 넣으면 `/app/whole_module/run_api.py` 를 찾는것이 되므로 에러가 난다.

### .dockerignore 생성

```dockerfile
# 예시
__pycache__/
*.pyc
*.pyo
*.pyd
*.db
.env
.git
```

### 이미지 build

```bash
cd mobility_module
docker build -t mobility_api .
docker build --no-cache -t <image_name> . # 레이어 캐시 무시 
```

### 컨테이너 실행

```bash
docker run --rm mobility_api
docker run -p <port>:<port> mobility_api
docker run --rm -p 7010:7010 mobility_api
# --rm : 프로세스가 끝나면 컨테이너 정보를 삭제하는 것. 
docker run -v /host/path/data:/app/data -p 7010:7010 mobility_api
# 볼륨 마운트를 통해 외부 파일 연결하기
```

### docker image 확인

```bash
docker images
```

### docker image 삭제

```bash
docker rmi mobility_api # 이름으로 삭제
docker rmi a1b2c3d3e4f5 # ID 로 삭제
```

### 컨테이너

- 확인

  ```bash
  docker ps -a
  ```

- 중지&삭제

  ```bash
  docker stop <ID>
  docker rm <ID>
  ```

### 데이터 출입에 대한 정의

데이터의 값 자체가 아예 API 나 그외 입출력방법에 의해 모듈에 들어오는 경우는 문제가 없지만
모듈 자체에서 어딘가에 저장되어있는 데이터를 불러오려는 경우, 해당 데이터는 docker 이미지에 포함되어있어야만 불러올 수 있음

예:) 데이터값 자체를 API 모듈에 입력 = 문제없음

데이터가 존재하는 path 정보를 api 모듈에 입력 후, API 모듈 내부에서 해당 데이터를 읽기 --> docker 이미지에 해당 데이터가 포함되어있지 않으면 읽기 불가능

```bash
docker run -v <데이터경로>:<데이터경로> -p 7010:7010 mobility_api
```

### 파일 저장시

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

### 디버깅(임시)

```bash
docker run -it mobility_api bash
```

### docker 캐시 제어

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

### 이미지 저장(export)

```bash
# 단순 추출
docker save -o <my_image>.tar <my_image>:<tag>
# 압축하는 경우
docker save <myapp>:<tag> | gzip > <myapp>.tar.gz
# 진행률 확인
docker save <myapp>:<tag> | pv | gzip > <myapp>.tar.gz

```

tag 는 의미 그대로 태그를 의미하며 명시하고자하는 버전 등을 적으면 됨 (예: latest)

### 이미지 불러오기

```bash
docker load -i <myapp>.tar
# 압축된 파일의 경우
gunzip -c <myapp>.tar.gz | docker load
# 진행률 확인
gunzip -c <myapp>.tar.gz | pv | docker load
```

### \<none> 삭제

```bash
docker image prune
```

